---
title: "dotnet: Azure analytics"
datePublished: Fri May 03 2024 06:42:52 GMT+0000 (Coordinated Universal Time)
cuid: clvqb5ybk000c08jrg32m0d6z
slug: dotnet-azure-analytics
tags: dotnet

---

* AppTraces: all the logs, such as warnings, information and error logs
    
* AppMetrics: telemetry metrics
    
* AppEvents: telemetry events
    

---

Which analytics to use?

* Logs are much more expensive to store compared to telemetry metrics and events
    
* Have a think about whether telemetry is good enough
    
* Key questions to think through
    
    * Why do I want to analyse this?
        
    * How will I know if it's successful?
        
    * How will I know if it's failed? How will I debug if it's failed?
        
    * How many times will this be triggered?
        

---

You can configure the telemetry to sample certain types

* Telemetry metrics are not listed as an option for sampling. Shahn says these are never sampled, as sampling would render the metric useless, given it is a count
    
* `Types`
    
    * Event: telemetry event
        
    * Exception: error logs and other exceptions
        
    * Trace: all logs, such as warning and information logs
        

```plaintext
        /// <summary>
        /// Adds <see cref="SamplingTelemetryProcessor"/> to the given<see cref="TelemetryProcessorChainBuilder" />.
        /// </summary>
        /// <param name="builder">Instance of <see cref="TelemetryProcessorChainBuilder"/>.</param>
        /// <param name="samplingPercentage">Sampling Percentage to configure.</param>     
        /// <param name="excludedTypes">Semicolon separated list of types that should not be sampled. Allowed type names: Dependency, Event, Exception, PageView, Request, Trace.</param>   
        /// <param name="includedTypes">Semicolon separated list of types that should be sampled. All types are sampled when left empty. Allowed type names: Dependency, Event, Exception, PageView, Request, Trace.</param> 
        /// <return>Same instance of <see cref="TelemetryProcessorChainBuilder"/>.</return>
        public static TelemetryProcessorChainBuilder UseSampling(this TelemetryProcessorChainBuilder builder, double samplingPercentage, string excludedTypes = null, string includedTypes = null)
      
```

---