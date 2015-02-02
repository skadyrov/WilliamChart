WilliamChart
===============

WilliamChart is an Android Library to help the implementation of charts in android applications. For the ones that would like to contribute, my idea is not only to implement the conventional chart features but instead something that could be pleasant and intuitive while representing and visualizing data. I would prefer to keep charts simple and clean rather than overfeatured.

At the moment it provides:

* ``LineChartView``
* ``BarChartView``
* ``StackBarChartView``




It has been tested in Android 2.2 and above.

![Demo2][1]

<a href="https://play.google.com/store/apps/details?id=com.db.williamchartdemo"><img alt="Android app on Google Play" src="https://developer.android.com/images/brand/en_app_rgb_wo_45.png" />
</a>

[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-WilliamChart-lightgrey.svg?style=flat)](https://android-arsenal.com/details/1/769)

Usage
-----
Each chart type has common but also specific customization attributes. Add the View to your layout and configure it using styleable attributes or/and programmatically.


### Gradle build

To install the sample application to your device run the following task:

```
$ ./gradlew installDebug
```

To deploy the library to your local Maven repository run the following task:

```
$ ./gradlew install
```

Then, to use the library in your project add the following to your `build.gradle`:

```groovy
dependencies {
    compile 'com.db.williamchart:library:1.7.0'
}
```

### Gradle install to local Maven repository

To install the library to the local Maven repository, run the following task in library folder:

```
$ gradle clean build generateRelease
```


Chart (Common customization)
----------------------

To create a new chart that requires axis extend the class ``ChartView`` and implement the necessary abstract methods. I believe the data you get from those methods should be enough to draw whatever you feel like.

```xml

    <com.db.chart.view.ChartView
        xmlns:chart="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="dp"
        ...
        chart:chart_shadowDx="dp"
        chart:chart_shadowDy="dp"
        chart:chart_shadowRadius="dp"
        chart:chart_shadowColor="color"
        chart:chart_fontSize="dp"
        chart:chart_typeface="typeface"
        chart:chart_axisBorderSpacing="dp"
        chart:chart_axisThickness="dp"
        chart:chart_axisTopSpacing="dp"
        chart:chart_axisColor="color"
        chart:chart_labelColor="color"
    />

```

```java
    
    // Customize labels
    chart.setYLabels(NONE/OUTSIDE/INSIDE)
    chart.setXLabels(NONE/OUTSIDE/INSIDE)
    chart.setLabelColor(color)
    chart.setLabelsMetric(string)
    chart.setFontSize(integer)
    chart.setTypeface(typeface)

    // Define grid
    chart.setGrid(gridtype, paint)
    
    // Show threshold line
    chart.setThresholdLine(float, paint)
	
	// Y Axis border labels
    chart.setAxisBorderValues(integer, integer, integer)
    // Step value between Y labels
    chart.setStep(integer)
    
    // Y Axis top spacing
    chart.setTopSpacing(dimen)
    
    // X Axis side spacing
    chart.setBorderSpacing(dimen)
    
    chart.setAxisX(boolean)
    chart.setAxisY(boolean)
    
    // Show both chart and its data
    chart.show(opt:index, opt:animation)
    
    // Dismiss data from chart
    chart.dismiss(opt:animation)
    chart.dismiss(index)
    
    // Update values of a given set
    chart.updateValues(int, array)
    // Notify chart about updated values
    chart.notifyDataUpdate()

    // Tooltip support
    chart.showTooltip(view, opt:boolean)
    chart.dismissTooltip(view)
    
    // Movement effect
    chartView.animateSet(index)
    
```


LineChart
----------

```xml
    <com.db.chart.LineChartView
        ... 
    />
```

```java
    LineChartView chartView= new LineChartView()

    LineSet lineSet = new LineSet()
    lineSet.addPoint(new Point(string, float)
    lineSet.addPoint(string, float)
    lineSet.addPoints(string[], float[])
    
    // Style dots
    lineSet.setDots(boolean)
    lineSet.setDotsColor(color)
    lineSet.setDotsRadius(dimen)
    lineSet.setDotsStrokeThickness(dimen)
    lineSet.setDotsStrokeColor(color)
    
    // Style line
    lineSet.setLineThickness(dimen)
    lineSet.setLineColor(color)
    
    // Style background fill
    lineSet.setFill(boolean)
    lineSet.setFillColor(color)
    
    // Style type
    lineSet.setDashed(boolean)
    lineSet.setSmooth(boolean)
    
    // Between which index should the line be displayed 
    lineSet.beginAt(index)
    lineSet.endAt(index)

    chartView.addData(lineSet)
    
```


BarChart & StackBarChart
-------------------------

```xml
    <com.db.chart.BarChartView
        ... 
        chart:chart_barSpacing="dp"
        chart:chart_setSpacing="dp"
    />
```

```java
    BarChartView chartView = new BarcChartView()
    barChart.setBarSpacing(dimen)
    barChart.setSetSpacing(dimen)
    barChart.setBarBackground(boolean)
    barChart.setBarBackgroundColor(color)
    barChart.setRoundCorners(dimen)

    BarSet barSet = new BarSet()
    barSet.addBar(string, float)
    barSet.addBars(string[], float[])

    Bar bar = new Bar(string, float)
    bar.setColor(color)
    barSet.addBar(bar)

    chartView.addData(barSet)
```



Listener
---------

```java
    chart.setOnEntryClickListener(new OnEntryClickListener(){
        @Override
        public void onClick(int setIndex, int entryIndex, Rect entryRect) {
            //Do things
        }
    });
```


Animation
---------

```java
    Animation anim = new Animation()

    anim.setDuration(integer)
    anim.setEasing(easingFunction)
    anim.setEndAction(runnable)
    
    // Animation overlap between entries
    anim.setOverlap(float)
    // Animation overlap between entries in a specific order
    anim.setOverlap(float,int[])
    // Animation starting point
    anim.setStartPoint(float, float)
    // Include alpha transition
    anim.setAlpha(int)
    
    // Starts animation
    chart.animate(animation)
```

Implementing the interface ``BaseEasingMethod`` you can create your own easing function. I've implemented a few (credits to [Jesus Gollonet](http://jesusgollonet.com/)):

* ``LinearEase``
* ``BounceEaseOut``
* ``ElasticEaseOut``
* ``CircEaseOut``
* ``CubicEaseOut``
* ``ExpoEaseOut``
* ``QuadEaseOut``
* ``QuartEaseOut``
* ``QuintEaseOut``
* ``SineEaseOut``


StyleAnimation
--------------
By extending ``BaseStyleAnimation`` and implementing the abstract ``nextUpdate(set)`` method you can animate any set's attribute (e.g. set's thickness). Be creative! So far these are the style-animations provided:

* ``DashAnimation``

License
-------

    Copyright 2015 Diogo Bernardino

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.



[1]: ./art/demo2.gif
