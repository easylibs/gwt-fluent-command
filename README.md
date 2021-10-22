# Fluent Comman
Fluent Command provides implementation of the command pattern discussed in best practices at [Google I/O event](https://www.youtube.com/watch?v=PDuhR18-EdM). 

You can send and execute commands remotely. As the library name suggests, a fluent or method chaining style of programming is implemented. 

*Example 1: - commands sent using GWT EventBus, works on client and server*
```java
package org.easylibs.gwt.command.examples.client;

import static org.easylibs.gwt.command.base.shared.testing.PingCommand.ping;

import java.util.logging.Logger;

import org.easylibs.gwt.command.base.shared.testing.PingHandler;
import org.easylibs.gwt.command.event.shared.CommandEventBus;
import org.easylibs.gwt.command.event.shared.EventCommand;

import com.google.gwt.event.shared.EventBus;

public class ExampleProgressMonitor {

	private final static Logger log = Logger.getLogger("examples");

	public static void main(String[] args) {
		log.info("ExampleProgressMonitor");

		EventBus bus = new CommandEventBus();
		bus.addHandler(EventCommand.type(), new PingHandler("EventBus: "));

		ping("cmd1", bus).onSuccess(log::info)
				.executeAfter(ping("cmd2", bus).onSuccess(log::info))
				.executeAfter(ping("cmd3", bus).onSuccess(log::info)
						.executeAfter(ping("cmd4", bus).onSuccess(log::info))
						.executeAfter(ping("cmd5", bus).onSuccess(log::info)))
				.execute();
	}
}
```
