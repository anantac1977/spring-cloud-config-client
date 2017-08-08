This project is to demonstrate the use of a Config client application.

The config client is responsible for bootstrapping and fetching the application configuration. There are two different ways that one can get the config client to bootstrap an application's properties.
Both of these ways use one special configuration file called as bootstrap.properties or bootstrap.yml.

1) config first: By configuring a bootstrap.yml/bootstrap.properties that has the application name and the url to the configuration server.

2) Discovery first: This approach uses the Discovery service. With this approach one needs to configure the bootstrap.properties/bootstrap.yml to have the application name and the location of the service Discovery server. In turn the discovery service would be used to find the location of the config server.

In order to quickly bootstrap it, go to http://start.spring.io/ and use the Spring Initializr to stub it out.
Generate it with the dependencies Eureka Discovery(optional), Config client and the Actuator.

Refreshing application configuration dynamically:
Steps involved:

1) Commit the configuration changes into Git, where the config server looks at.
2) There are 3 different ways that the application gets the new configurations:
	i) Manually: by calling the /refresh endpoint of spring-actuator. But doing this for every individual service that needs a configuration update, is cumbersome.
	ii) Manual + Automatic(bus/refresh with spring-cloud-bus): All application services need to
	 register with spring-bus for an event and to call /bus/refresh endpoint when that event occurs.
	 But with this approach an application service needs to invoke this endpoint regardless of whether
	 an update to it's configuration is required or not.
	iii) vcs+/monitor with spring-cloud-config-monitor and spring-cloud-bus: The change set of a Git 
	commit is posted to a monitor endpoint. The monitor endpoint would determine which services need 
	update their configurations.
	
Anything annotated with @Bean and @Value will not get updated with any dynamic config changes, using any pf the above approaches. These get their configuration properties only at the time of their initialization. @Bean and @Value annotated variables can be updated with the new configuration by using @RefreshScope annotation.