# Generic Measure Monitoring Plugin

**Description** Opens an HTTP Listener Port with which you can feed ANY generic measure to Dynatrace. One of the use cases will be feeding data from JMeter or other testing tools to Dynatrace

**Name and Version** Generic Measure Monitoring Plugin

**Compatible with** Dynatrace 6.3+

**Author** Andreas Grabner (Dynatrace)

**License** Dynatrace Experimental Software License

**Support Level** Not Supported

**Download (latest version)** [com.dynatrace.community_1.0.0.jar](https://github.com/Dynatrace/Dynatrace-Generic-Measure-Monitoring-Plugin/releases/download/v1.0%2C0/com.dynatrace.community_1.0.0.jar)

## Description
It allows you to push measure data to Dynatrace via a REST API. Here are some sample URLs that will create measures in Dynatrace

```
URL Format
[SplitName:]MeasureName1=value[,MeasureName2=value][;SplitName2:MeasureName=Value]

Sample URLs that will deliver data:
http://localhost:4000/genericmonitor?Timer1:Response Time=123;Timer2:Response Time=323
This will generate a Response Time Measure with two Split Values "Timer 1" and "Timer 2" with the values 123 and 323
http://localhost:4000/genericmonitor?Timer1:Response Time=123,Bytes=234,Latency=332
http://localhost:4000/genericmonitor?Response Time=123,Latency=333
http://localhost:4000/genericmonitor?Response Time=123
```

One of the reasons I built this plugin was to allow tools such as JMeter to stream the results to Dynatrace so that you can automatically see the JMeter measures in Dynatrace on the same dashboard as where you have all your Dynatrace App Metrics to analyze what is actually going on in your load test. 

So - here is a sample JMeter BeanShell Listener that is forwarding JMeter results to the Generic Measure Monitoring Plugin which listens on http://mycollector:4000/genericmonitor

![](https://github.com/Dynatrace/Dynatrace-Generic-Measure-Monitoring-Plugin/blob/master/images/JMeterSample.png)

And here is now how this could look like when you take this data and put it on a Dynatrace Web Dashboard with all your other Dynatrace Metrics

![](https://github.com/Dynatrace/Dynatrace-Generic-Measure-Monitoring-Plugin/blob/master/images/JMeterDTWebDashboard.png)

# Installation 
Download the plugin from GitHub and then Install it as you would install any other plugin in Dynatrace. Check out the documentation for instructions: [Plugin Management](https://community.dynatrace.com/community/display/DOCDT65/Plugin+Management)

**The plugin will open an HTTP Listening Port on the Collector it runs. Default is 4000 but this can be changed**

# Configuring the Plugin

First you want to download the latest release and install the plugin on your Dynatrace Server. Simply import it via the Settings -> Dynatrace Server -> Plugins Tab in your Dynatrace Client. Then go ahead and create a new Monitor in your System Profile. Select "Generic Measure Monitoring Plugin" as the Monitor Type:

## Step 1: Configure the port & endpoint
For the host - simply select any ONE host. This host doesnt matter for the plugin as we are just listening. Key is that you only select ONE host. I typically pick my local machine or any machine where the Dynatrace server runs:
![](https://github.com/Dynatrace/Dynatrace-Generic-Measure-Monitoring-Plugin/blob/master/images/PluginConfigStep1.png)

## Step 2: Select your Collector and Frequency
As the plugin runs on a Collector it will be this Collector that will open up that listening port. So - make sure you pick a Collector that doesnt block this port and that is accessible from your tools, e.g: JMeter that will feed data to that machine
If you want to have very granular data make sure you select "10s" as schedule!
![](https://github.com/Dynatrace/Dynatrace-Generic-Measure-Monitoring-Plugin/blob/master/images/PluginConfigStep2.png)

## Step 3: Configure the Numeric Metric
All the plugin does right now is delivering numeric metrics. So - simply go with the default metric that is listed!
![](https://github.com/Dynatrace/Dynatrace-Generic-Measure-Monitoring-Plugin/blob/master/images/PluginConfigStep3.png)

Once you are done the plugin starts listening on the configured port

# Test the plugin

You will also find the JMeter script in this github project I used in the first screenshot. So - either use that script which tests a bunch of news websites or simply try to execute some of the sample URLs I gave you in the beginning. Easiest way to test is by just entering that URL in your browser:

![](https://github.com/Dynatrace/Dynatrace-Generic-Measure-Monitoring-Plugin/blob/master/images/TestItViaBrowser.png)

Once you have sent some data over to Dynatrace you should be able to select these metrics when putting them on a chart:
![](https://github.com/Dynatrace/Dynatrace-Generic-Measure-Monitoring-Plugin/blob/master/images/MeasuresInDynatrace.png)

And you should start seeing data in your dashboards

![](https://github.com/Dynatrace/Dynatrace-Generic-Measure-Monitoring-Plugin/blob/master/images/MeasuresInCharts.png)

If you have more questions let me know.
