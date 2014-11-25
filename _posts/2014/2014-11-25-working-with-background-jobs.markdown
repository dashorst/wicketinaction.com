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

From time to Wicket users ask questions related to how to deal with background jobs in Wicket. E.g. "How do I make Application or Session available 
to a background thread?" Or, "How do I deal with showwing some progress information, or allow the user to cancel a backgroung process?". We have build a small toy 
project to illstrate a posible way to do it. We hope this small project could help people getting started to roll their own solutions.

The complete code of the application can be found at: <a target="_blank" href="https://github.com/reiern70/antilia-bits/tree/master/bgprocess">https://github.com/reiern70/antilia-bits/tree/master/bgprocess</a>. Feel free to just grab the code and use it in any way you please.


# The solution

We start by defining an interface that represents a Taks. i.e. some lengthy computation to be done in a background non-WEB thread.

public interface ITask extends Serializable {

	void doIt(ExecutionBridge bridge);
}

the method doIt receives a context/bridge class that it can use to communicate with the WEB layer. 

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

As an example we provide a dummy task, that looks like.


public class DummyTask implements ITask {

	private static final Logger LOGGER = LoggerFactory.getLogger(DummyTask.class);

	@javax.inject.Inject
	private IAnswerService answerService;
	
	public DummyTask() {
		Injector.get().inject(this);
	}
	
	@Override
	public void doIt(ExecutionBridge bridge) {

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

This task does nothing but iterate and call a service, IAnswerService, to get some text messages to pass to the WEB layer via ExecutionBridge. This class also uses information 
contained on bridge to determine if the task should be stoped or cancelled. Mind that the task uses Wicket injection machinery to inject an implementation of IAnswerService, so, 
that mean we need to have Application as a thread local when Injector.get().inject(this); is called. This is achieved by providing a runnable that beside executing tasks, will
make sure Application and Session are attached, and properly detached, as thread locals. 

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
		try
		{
			ThreadContext.setApplication(application);
			ThreadContext.setSession(session);
			task.doIt(bridge);
		} finally
		{
			ThreadContext.detach();
		}
	}
}

Additionally we provide custom Session and Application classes containing the machinery to track user's running tasks and launch tasks. See

public class BgProcessApplication extends WebApplication
{
	ExecutorService executorService =  new ThreadPoolExecutor(10, 10,
			0L, TimeUnit.MILLISECONDS,
			new LinkedBlockingQueue<Runnable>());

	@Override
	public Class<? extends WebPage> getHomePage()
	{
		return HomePage.class;
	}

	@Override
	public void init()
	{
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

	public static BgProcessApplication getApplication() {
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

and 

public class BgProcessSession extends WebSession {

	private List<ExecutionBridge> bridges = new ArrayList<ExecutionBridge>();

	public BgProcessSession(Request request) {
		super(request);
	}

	public void add(ExecutionBridge bridge) {
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
		// mgrigorov: maybe make a copy of the subList to avoid ConcurrentModificationExceptions ?!
		return bridges.subList(start, min).iterator();
	}

	public long countTasks() {
		return bridges.size();
	}

	public static BgProcessSession getSession() {
		return (BgProcessSession)get();
	}
}

## The WEB layer to handle/manage taks.


# Final words


