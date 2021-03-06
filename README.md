# PNChart

**PNChart** for OS X (Cocoa).

You can also find [PNChart](https://github.com/kevinzhow/PNChart) or [PNChart-Swift](https://github.com/kevinzhow/PNChart-Swift) for iOS (CocoaTouch).

![PNChart](https://raw.githubusercontent.com/tracy-e/PNChart/master/Images/PNChart.gif)


## Requirements

PNChart works on OS X 10.10 and later version and is compatible with ARC projects. It depends on the following Apple frameworks, which should already be included with most Xcode templates:

* Cocoa.framework
* CoreGraphics.framework
* QuartzCore.framework

You will need LLVM 3.0 or later in order to build PNChart.


## Usage

### Copy the PNChart folder to your project

![Line Chart](https://raw.githubusercontent.com/tracy-e/PNChart/master/Images/LineChart.png)

```objective-c
#import "PNChart.h"

//For Line Chart
PNLineChart * lineChart = [[PNLineChart alloc] initWithFrame:CGRectMake(20, 100, 360, 200.0)];
[lineChart setXLabels:@[@"SEP 1",@"SEP 2",@"SEP 3",@"SEP 4",@"SEP 5"]];

// Line Chart No.1
NSArray * data01Array = @[@60.1, @160.1, @126.4, @262.2, @186.2];
PNLineChartData *data01 = [PNLineChartData new];
data01.color = PNFreshGreen;
data01.itemCount = lineChart.xLabels.count;
data01.getData = ^(NSUInteger index) {
    CGFloat yValue = [data01Array[index] floatValue];
    return [PNLineChartDataItem dataItemWithY:yValue];
};
// Line Chart No.2
NSArray * data02Array = @[@20.1, @180.1, @26.4, @202.2, @126.2];
PNLineChartData *data02 = [PNLineChartData new];
data02.color = PNTwitterColor;
data02.itemCount = lineChart.xLabels.count;
data02.getData = ^(NSUInteger index) {
    CGFloat yValue = [data02Array[index] floatValue];
    return [PNLineChartDataItem dataItemWithY:yValue];
};

lineChart.chartData = @[data01, data02];
[lineChart strokeChart];

```

![Bar Chart](https://raw.githubusercontent.com/tracy-e/PNChart/master/Images/BarChart.png)

```objective-c
#import "PNChart.h"

//For Bar Chart
PNBarChart * barChart = [[PNBarChart alloc] initWithFrame:CGRectMake(50, 135.0, 320, 200.0)];
[barChart setXLabels:@[@"SEP 1",@"SEP 2",@"SEP 3",@"SEP 4",@"SEP 5"]];
[barChart setYValues:@[@1,  @10, @2, @6, @3]];
[barChart strokeChart];

```

![Circle Chart](https://raw.githubusercontent.com/tracy-e/PNChart/master/Images/CircleChart.png)

```objective-c
#import "PNChart.h"

//For Circle Chart

PNCircleChart * circleChart = [[PNCircleChart alloc] initWithFrame:[[PNCircleChart alloc] initWithFrame:CGRectMake(0, 150, CGRectGetWidth(self.view.bounds), 150) total:@100 current:@80 clockwise:YES shadow:YES];
[circleChart setStrokeColor:PNGreen];
[circleChart setStrokeColorGradientStart:PNBlue];
[circleChart strokeChart];

```


![Pie Chart](https://raw.githubusercontent.com/tracy-e/PNChart/master/Images/PieChart.png)

```objective-c
# import "PNChart.h"

//For Pie Chart
NSArray *items = @[[PNPieChartDataItem dataItemWithValue:10 color:PNRed],
                           [PNPieChartDataItem dataItemWithValue:20 color:PNBlue description:@"WWDC"],
                           [PNPieChartDataItem dataItemWithValue:40 color:PNGreen description:@"GOOL I/O"],
                           ];



PNPieChart *pieChart = [[PNPieChart alloc] initWithFrame:CGRectMake(100, 100, 200.0, 200.0) items:items];
pieChart.descriptionTextColor = [NSColor whiteColor];
pieChart.descriptionTextFont  = [NSFont fontWithName:@"Avenir-Medium" size:14.0];
[pieChart strokeChart];

```

![Scatter Chart](https://raw.githubusercontent.com/tracy-e/PNChart/master/Images/ScatterChart.png)

```objective-c
# import "PNChart.h"
//For Scatter Chart

PNScatterChart *scatterChart = [[PNScatterChart alloc] initWithFrame:CGRectMake(SCREEN_WIDTH /6.0 - 30, 135, 280, 200)];
[scatterChart setAxisXWithMinimumValue:20 andMaxValue:100 toTicks:6];
[scatterChart setAxisYWithMinimumValue:30 andMaxValue:50 toTicks:5];

NSArray * data01Array = [self randomSetOfObjects];
PNScatterChartData *data01 = [PNScatterChartData new];
data01.strokeColor = PNGreen;
data01.fillColor = PNFreshGreen;
data01.size = 2;
data01.itemCount = [[data01Array objectAtIndex:0] count];
data01.inflexionPointStyle = PNScatterChartPointStyleCircle;
__block NSMutableArray *XAr1 = [NSMutableArray arrayWithArray:[data01Array objectAtIndex:0]];
__block NSMutableArray *YAr1 = [NSMutableArray arrayWithArray:[data01Array objectAtIndex:1]];
data01.getData = ^(NSUInteger index) {
    CGFloat xValue = [[XAr1 objectAtIndex:index] floatValue];
    CGFloat yValue = [[YAr1 objectAtIndex:index] floatValue];
    return [PNScatterChartDataItem dataItemWithX:xValue AndWithY:yValue];
};

[scatterChart setup];
self.scatterChart.chartData = @[data01];
/***
this is for drawing line to compare
CGPoint start = CGPointMake(20, 35);
CGPoint end = CGPointMake(80, 45);
[scatterChart drawLineFromPoint:start ToPoint:end WithLineWith:2 AndWithColor:PNBlack];
***/
scatterChart.delegate = self;
```

#### Update Value

Now it's easy to update value in real time

```objective-c
if ([self.title isEqualToString:@"Line Chart"]) {

    // Line Chart #1
    NSArray * data01Array = @[@(arc4random() % 300), @(arc4random() % 300), @(arc4random() % 300), @(arc4random() % 300), @(arc4random() % 300), @(arc4random() % 300), @(arc4random() % 300)];
    PNLineChartData *data01 = [PNLineChartData new];
    data01.color = PNFreshGreen;
    data01.itemCount = data01Array.count;
    data01.inflexionPointStyle = PNLineChartPointStyleTriangle;
    data01.getData = ^(NSUInteger index) {
        CGFloat yValue = [data01Array[index] floatValue];
        return [PNLineChartDataItem dataItemWithY:yValue];
    };

    // Line Chart #2
    NSArray * data02Array = @[@(arc4random() % 300), @(arc4random() % 300), @(arc4random() % 300), @(arc4random() % 300), @(arc4random() % 300), @(arc4random() % 300), @(arc4random() % 300)];
    PNLineChartData *data02 = [PNLineChartData new];
    data02.color = PNTwitterColor;
    data02.itemCount = data02Array.count;
    data02.inflexionPointStyle = PNLineChartPointStyleSquare;
    data02.getData = ^(NSUInteger index) {
        CGFloat yValue = [data02Array[index] floatValue];
        return [PNLineChartDataItem dataItemWithY:yValue];
    };

    [self.lineChart setXLabels:@[@"DEC 1",@"DEC 2",@"DEC 3",@"DEC 4",@"DEC 5",@"DEC 6",@"DEC 7"]];
    [self.lineChart updateChartData:@[data01, data02]];

}
else if ([self.title isEqualToString:@"Bar Chart"])
{
    [self.barChart setXLabels:@[@"Jan 1",@"Jan 2",@"Jan 3",@"Jan 4",@"Jan 5",@"Jan 6",@"Jan 7"]];
    [self.barChart updateChartData:@[@(arc4random() % 30),@(arc4random() % 30),@(arc4random() % 30),@(arc4random() % 30),@(arc4random() % 30),@(arc4random() % 30),@(arc4random() % 30)]];
}
else if ([self.title isEqualToString:@"Circle Chart"])
{
    [self.circleChart updateChartByCurrent:@(arc4random() % 100)];
}
```

#### Callback


```objective-c
#import "PNChart.h"

//For LineChart

lineChart.delegate = self;


```

```objective-c

//For DelegateMethod


-(void)userClickedOnLineKeyPoint:(CGPoint)point lineIndex:(NSInteger)lineIndex pointIndex:(NSInteger)pointIndex{
    NSLog(@"Click Key on line %f, %f line index is %d and point index is %d",point.x, point.y,(int)lineIndex, (int)pointIndex);
}

-(void)userClickedOnLinePoint:(CGPoint)point lineIndex:(NSInteger)lineIndex{
    NSLog(@"Click on line %f, %f, line index is %d",point.x, point.y, (int)lineIndex);
}

```


## License

This code is distributed under the terms and conditions of the [MIT license](LICENSE).

