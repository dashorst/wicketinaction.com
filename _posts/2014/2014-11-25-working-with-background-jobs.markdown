---
layout: post
status: publish
published: true
title: Working with background jobs
author: reiern70
date: '2014-07-01 10:00:00 +0300'
date_gmt: '2014-07-01 08:00:00 +0100'
categories:
- Wicket
tags: [background, jobs, wicket]
comments: []
---

# The problem 

From time to time Wicket users ask questions related to how to deal with background jobs in Wicket. E.g. "How do I make Application or Session available 
to a background thread?" Or, "How do I deal with showing some progress information, or allow the user to cancel a background process?". We have build a small toy 
project to illustrate a possible way to do those things. We hope this project could help people get started on rolling their own solutions.

The complete code of the application can be found at: <a target="_blank" href="https://github.com/reiern70/antilia-bits/tree/master/bgprocess">https://github.com/reiern70/antilia-bits/tree/master/bgprocess</a>. Feel free to just grab the code and use it in any way you please.


# The solution

We start by defining an interface that represents a Task. i.e. some lengthy computation to be done in a background non-WEB thread.

{% highlight java %}
public interface ITask extends Serializable {

	void doIt(ExecutionBridge bridge);
}
{% endhighlight %}

the method doIt() receives a context/bridge class that can be used to communicate with the WEB layer. 

{% highlight java %}
public class ExecutionBridge implements Serializable {

	private String taskName;

	private volatile boolean stop = false;

	private volatile boolean cancel = false;

	private volatile int progress = 0;

	private String message;

	public ExecutionBridge() {
	}

	... setter and gettters
}
{% endhighlight %}

As an example we provide a dummy task, that looks like.

{% highlight java %}
public class DummyTask implements ITask {

	private static final Logger LOGGER = LoggerFactory.getLogger(DummyTask.class);

	@javax.inject.Inject
	private IAnswerService answerService;
	
	public DummyTask() {
		Injector.get().inject(this);
	}
	
	@Override
	public void doIt(ExecutionBridge bridge) {

                // check thread locals are available
		LOGGER.info("App: {}", Application.exists());
		LOGGER.info("Session: {}", Session.exists());

		for(int i =1; i <= 100; ) {
			bridge.setProgress(i);
			if(bridge.isCancel()) {
				bridge.setMessage(answerService.getMessage(AnswerType.IS_CANCEL, i));
				// end the task!
				return;
			}
			if(bridge.isStop()) {
				bridge.setMessage(answerService.getMessage(AnswerType.IS_STOP, i));
			} else {
				bridge.setMessage(answerService.getMessage(AnswerType.ALL_OK, i));
				i++;
			}
			try {
				Thread.sleep(1000L);
			} catch (InterruptedException e) {
				bridge.setProgress(100);
				bridge.setMessage(answerService.getMessage(AnswerType.IS_ERROR, i));
				return;
			}
		}
	}
}
{% endhighlight %}

This task does nothing but iterate from 1 to 100 and repeatedly call a service, IAnswerService, to get some text messages to pass to the WEB layer via the ExecutionBridge instance. 
This class also uses information contained on bridge to determine if the task should be stopped or canceled. Mind that the task uses Wicket injection machinery to inject an implementation of IAnswerService, so, that mean we need to have Application as a thread local when Injector.get().inject(this); is called. This is achieved by providing a runnable that beside executing tasks, will
make sure Application and Session are attached, and properly detached, as thread locals. 

{% highlight java %}
public class TasksRunnable implements Runnable {

	private final ITask task;
	private final ExecutionBridge bridge;
	private final Application application;
	private final Session session;

	public TasksRunnable(ITask task, ExecutionBridge bridge) {
		this.task = task;
		this.bridge = bridge;

		this.application = Application.get();
		this.session = Session.exists() ? Session.get() : null;
	}

	@Override
	public void run() {
		try {
			ThreadContext.setApplication(application);
			ThreadContext.setSession(session);
			task.doIt(bridge);
		} finally {
			ThreadContext.detach();
		}
	}
}
{% endhighlight %}

Additionally, we provide custom Session and Application classes containing the machinery to track user's running tasks and to launch tasks, respectively. See

{% highlight java %}
public class BgProcessApplication extends WebApplication {
	ExecutorService executorService =  new ThreadPoolExecutor(10, 10,
			0L, TimeUnit.MILLISECONDS,
			new LinkedBlockingQueue<Runnable>());

	@Override
	public Class<? extends WebPage> getHomePage() {
		return HomePage.class;
	}

	@Override
	public void init() {
		super.init();
		com.google.inject.Injector injector = Guice.createInjector(newModule());
		getComponentInstantiationListeners().add( new GuiceComponentInjector(this, injector));
	}

	@Override
	protected void onDestroy() {
		executorService.shutdown();
		try {
			if (!executorService.awaitTermination(2, TimeUnit.SECONDS)) {
				executorService.shutdownNow();
			}
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		super.onDestroy();
	}

	private Module newModule() {
		return new Module() {
				public void configure(Binder binder) {
					binder.bind(IAnswerService.class).to(AnswerService.class);
				}
			};
	}

	public static BgProcessApplication get() {
		return (BgProcessApplication)get();
	}

	@Override
	public Session newSession(Request request, Response response) {
		return new BgProcessSession(request);
	}

	public void launch(ITask task) {
		// we are on WEB thread so services should be normally injected.
		ExecutionBridge bridge = new ExecutionBridge();
		// register bridge on session
		bridge.setTaskName("Task-" + BgProcessSession.getSession().countTasks() + 1);
		BgProcessSession.getSession().add(bridge);
		// run the task
		executorService.execute( new TasksRunnable(task, bridge));
	}
} 
{% endhighlight %}

and 

{% highlight java %}
public class BgProcessSession extends WebSession {

	private List<ExecutionBridge> bridges = new ArrayList<ExecutionBridge>();

	public BgProcessSession(Request request) {
		super(request);
	}

	public synchronized void add(ExecutionBridge bridge) {
		bind();
		bridges.add(bridge);
	}

	public synchronized void  pruneFinishedTasks() {
		ArrayList<ExecutionBridge> nonFinishedBridges = new ArrayList<ExecutionBridge>();
		for(ExecutionBridge bridge: this.bridges) {
			if (!bridge.isFinished()) {
				nonFinishedBridges.add(bridge);
			}
		}
		this.bridges = nonFinishedBridges;
	}

	public Iterator<ExecutionBridge> getTasksPage(int start, int size) {
		int min = Math.min(size, bridges.size());
		return new ArrayList<ExecutionBridge>(bridges.subList(start, min)).iterator();
	}

	public long countTasks() {
		return bridges.size();
	}

	public static BgProcessSession get() {
		return (BgProcessSession)get();
	}
}
{% endhighlight %}

## The WEB layer to handle/manage tasks.

The WEB layer is more or less standard Wicket. The more complex class is TasksListPanel which uses an AjaxFallbackDefaultDataTable in order to display running tasks. This panel, contains an
AjaxSelfUpdatingTimerBehavior that takes care of repainting the panel to show tasks progress. There are other user interactions, like creating new tasks and pruning "dead" tasks, but we will
not get into the details.

{% highlight java %}
public class TasksListPanel extends Panel {
	/**
	 * @param id
	 */
	public TasksListPanel(String id) {
		super(id);
		setOutputMarkupId(true);
		add( new AjaxLink<Void>("prune") {
			@Override
			public void onClick(AjaxRequestTarget target) {
				BgProcessSession.getSession().pruneFinishedTasks();
				target.add(TasksListPanel.this);
			}
		});
		add( new AjaxSelfUpdatingTimerBehavior( Duration.seconds(5) ) );
		ArrayList<IColumn<ExecutionBridge, String>> columns = new ArrayList<IColumn<ExecutionBridge,String>>();
		columns.add( new PropertyColumn<ExecutionBridge, String>(Model.of("Actions"), "-") { 
			@Override
			public void populateItem(
					Item<ICellPopulator<ExecutionBridge>> item,
					String componentId, IModel<ExecutionBridge> rowModel) {
				item.add( new ActionsPanel(componentId, rowModel) {
					@Override
					public void onAction(AjaxRequestTarget target) {
						TasksListPanel.this.add(new AjaxSelfUpdatingTimerBehavior( Duration.seconds(5)));
						target.add(TasksListPanel.this);
					}
				});
			}
		});
		columns.add( new PropertyColumn<ExecutionBridge, String>(Model.of("Name"), "taskName") );
		columns.add( new PropertyColumn<ExecutionBridge, String>(Model.of("Stoped"), "stop") );
		columns.add( new PropertyColumn<ExecutionBridge, String>(Model.of("Canceled"), "cancel") );
		columns.add( new PropertyColumn<ExecutionBridge, String>(Model.of("Message"), "message") );
		columns.add( new PropertyColumn<ExecutionBridge, String>(Model.of("Progress"), "progress") );
		columns.add( new PropertyColumn<ExecutionBridge, String>(Model.of("Finished"), "finished") );
		AjaxFallbackDefaultDataTable<ExecutionBridge, String> tasks = new AjaxFallbackDefaultDataTable<ExecutionBridge, String>("tasks", columns, new TaskDataProvider(), 10);
		add(tasks);
	}
	
	@Override
	public void onEvent(IEvent<?> event) {
		if(event.getPayload() instanceof TaskLaunchedEvent) {
			TaskLaunchedEvent event2 = (TaskLaunchedEvent)event.getPayload();
			TasksListPanel.this.add(new AjaxSelfUpdatingTimerBehavior( Duration.seconds(5)));
			event2.getTarget().add(TasksListPanel.this);
		}
	}
}
{% endhighlight %}

# Final words

We hope this example helps you get started in rolling your own solution. 

Last but not least, I would like to thanks Martin Grigorov for guiding me in writing this small article, making nice amendments to it and for he's invaluable work maintaining Apache Wicket.

