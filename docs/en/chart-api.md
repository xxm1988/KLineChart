# Chart API

## init(ds, options)
```typescript
(ds: string | HTMLElement, options?: {
   locale?: string,
   timezone?: string,
   styles?: string | object,
   customApi?: {
      formatDate?: (dateTimeFormat: Intl.DateTimeFormat, timestamp: number, format: string, type: number) => string,
      formatBigNumber?: (value: string | number) => string
   }
}) => Chart
```
Initialize a chart and return the chart instance.
- `ds` container, can be dom element or element id.
- `options` optional configuration items.
   - `locale` language, built-in support for `zh-CN` and `en-US`.
   - `timezone` time zone name, such as 'Asia/Shanghai', if not set, it will automatically get the local time zone, please refer to [timezone list](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones#List ).
   - `styles` It can be the style name registered through `klinecharts.registerStyles`, or it can be an object, a list of styles, see [styles](/guide/style) for details, and supports increments.
   - `customApi` customize some APIs.
     - `formatDate` formats a date.
     - `formatBigNumber` format big numbers, such as 1000 into 1k, 1000000 into 1M, etc.


## dispose(dcs)
```typescript
(dcs: HTMLElement | Chart | string) => void
```
Destroys a chart, once destroyed the chart will no longer be available.
- `dcs` can be a dom element, element id or chart instance.


## registerLocale(locale, locales)
```typescript
(
   locale: string,
   locales: {
     time: string,
     open: string,
     high: string,
     low: string,
     close: string,
     volume: string
   }
) => void
```
Add a localization language. Charts have built-in `zh-CN` and `en-US`.
- `locale` language name
- `locales` language configuration

## getSupportedLocales()
```typescript
() => string[]
```
Get the localized language types supported by the chart.

## registerStyles(name, styles)
```typescript
(
   name: string,
   styles: object
) => void
```
Add a style configuration.
- `name` style name
- `styles` style configuration, refer to [style](/guide/style) for the type, support increment.

## registerFigure(figure)
```typescript
(figure: {
   name: string,
   draw: (ctx: CanvasRenderingContext2D, attrs: any, styles: object) => void,
   checkEventOn: (coordinate: Coordinate, attrs: any, styles: object) => boolean
}) => void
```
Add a base shape.
- `figure` Basic figure information, see [figure](/advanced) for details
   - `name` name, unique identifier
   - `draw` drawing method
   - `checkEventOn` checks if the event is on the graph

## getSupportedFigures()
```typescript
() => string[]
```
Get the basic graph type supported by the graph.

## getFigureClass(name)
```typescript
(name: string) => Figure
```
Get graph class.
- `name` name

## registerIndicator(indicator)
```typescript
(indicator: {
   name: string,
   shortName?: string,
   precision?: number,
   calcParams?: any[],
   shouldOhlc?: boolean,
   shouldFormatBigNumber?: boolean,
   visible?: boolean,
   extendData?: any,
   series?: 'normal' | 'price' | 'volume',
   figures?: Array<{
      key: string,
      title?: string,
      type?: string,
      baseValue?: number,
      styles?: (
         data: object,
         indicator: object,
         defaultStyles: object
      ) => { style?: 'solid' | 'dashed' | 'stroke' | 'fill' | 'stroke_fill', color?: string }
   }>
   minValue?: number,
   maxValue?: number,
   styles?: object,
   calc: (dataList: KLineData[], indicator: object) => Promise<object[]> | object[],
   regenerateFigures?: (calcParms: any[]) => Array<{
      key: string,
      title?: string,
      type?: string,
      baseValue?: number,
      styles?: (
         data: object,
         indicator: object,
         defaultStyles: object
      ) => { style?: 'solid' | 'dashed' | 'stroke' | 'fill' | 'stroke_fill', color?: string }
   }>,
   createTooltipDataSource?: (params: object) => {
      name?: string,
      calcParamsText?: string,
      values?: Array<{
         title: string | { text: string, color: string },
         value: string | { text: string, color: string }
      }>
   },
   draw?: (params: object) => boolean
}) => void
```
Add a technical indicator.
- `indicator` technical indicator information
   - `name` indicator name, unique identifier for creation and operation
   - `shortName` short name for display
   - `precision` precision
   - `calcParams` calculation parameters
   - `shouldOhlc` needs ohlc auxiliary graphics
   - `shouldFormatBigNumber` should format large numbers. For example, 1000 is converted to 1k, 1000000 is converted to 1M, etc.
   - `visible` visible or not
   - `extendData` extended data
   - `series` indicator series, options are 'normal', 'price' and 'volume'
   - `figures` graphics configuration
   - `minValue` specifies the minimum value
   - `maxValue` specifies the maximum value
   - `styles` styles
   - `calc` calculation method
   - `regenerateFigures` method to regenerate figure information
   - `createTooltipDataSource` method to create custom tip information
   - `draw` custom drawing method


## getSupportedIndicators()
```typescript
() => string[]
```
Get technical indicators for chart support.

## registerOverlay(overlay)
```typescript
(overlay: {
   name: string,
   totalStep?: number,
   lock?: boolean,
   visible?: boolean,
   needDefaultPointFigure?: boolean,
   needDefaultXAxisFigure?: boolean,
   needDefaultYAxisFigure?: boolean,
   mode?: 'normal' | 'weak_magnet' | 'strong_magnet',
   points?: Array<{ timestamp: number, dataIndex?: number, value?: number }>,
   extendData?: any,
   styles?: object,
   createPointFigures?: (params: object) => {
      key?: string,
      type: string,
      attrs: any | any[],
      styles?: any,
      ignoreEvent?: boolean
   } | Array<{
      key?: string,
      type: string,
      attrs: any | any[],
      styles?: any,
      ignoreEvent?: boolean
   }>,
   createXAxisFigures?: (params: object) => {
      key?: string,
      type: string,
      attrs: any | any[],
      styles?: any,
      ignoreEvent?: boolean
   } | Array<{
      key?: string,
      type: string,
      attrs: any | any[],
      styles?: any,
      ignoreEvent?: boolean
   }>,
   createYAxisFigures?: (params: object) => {
      key?: string,
      type: string,
      attrs: any | any[],
      styles?: any,
      ignoreEvent?: boolean
   } | Array<{
      key?: string,
      type: string,
      attrs: any | any[],
      styles?: any,
      ignoreEvent?: boolean
   }>,
   performEventPressedMove?: (params: object) => void,
   performEventMoveForDrawing?: (params: object) => void,
   onDrawStart?: (event: object) => boolean,
   onDrawing?: (event: object) => boolean,
   onDrawEnd?: (event: object) => boolean,
   onClick?: (event: object) => boolean,
   onRightClick?: (event: object) => boolean,
   onPressedMoveStart?: (event: object) => boolean,
   onPressedMoving?: (event: object) => boolean,
   onPressedMoveEnd?: (event: object) => boolean,
   onMouseEnter?: (event: object) => boolean,
   onMouseLeave?: (event: object) => boolean,
   onRemoved?: (event: object) => boolean,
   onSelected?: (event: object) => boolean,
   onDeselected?: (event: object) => boolean
}) => void
```
Add a overlay.
- `overlay` overlay information, see [overlay](/advanced/overlay) for details
   - `name` overlay name, unique identifier for creation
   - `totalStep` total implementation steps
   - `lock` is locked to prevent dragging
   - `visible` visible or not
   - `needDefaultPointFigure` needs the default point figure
   - `needDefaultXAxisFigure` needs the default x-axis figure
   - `needDefaultYAxisFigure` needs the default y-axis figure
   - `mode` mode, options are 'normal', 'weak_magnet' and 'strong_magnet'
   - `points` point information
   - `extendData` extended data
   - `styles` styles
   - `createPointFigures` creates figures corresponding to points
   - `createXAxisFigures` creates figures on the x-axis
   - `createYAxisFigures` creates figures on the y-axis
   - `performEventPressedMove` special handling method for press and move event
   - `performEventMoveForDrawing` special processing method during moving event
   - `onDrawStart` start drawing event
   - `onDrawing` drawing event
   - `onDrawEnd` draw end event
   - `onClick` click event
   - `onRightClick` right click event
   - `onPressedMoveStart` press start move event
   - `onPressedMoving` Press and move event
   - `onPressedMoveEnd` Press and move end event
   - `onMouseEnter` mouse enter event
   - `onMouseLeave` mouse out event
   - `onRemoved` delete event
   - `onSelected` selected event
   - `onDeselected` deselected event

## getSupportedOverlays()
```typescript
() => string[]
```
Get overlays for chart support.

## version()
```typescript
() => string
```
Get the current version number of the chart.

## utils
A collection of helper methods.
### utils.clone(target)
```typescript
(target: any) => any
```
deep copy.

### utils.merge(target, source)
```typescript
(target: object, source: object) => void
```
Merge one object into another.

### utils.isString(value)
```typescript
(value: any) => boolean
```
Checks if a value is a string.

### utils.isNumber(value)
```typescript
(value: any) => boolean
```
Checks if a value is a number.

### utils.isValid(value)
```typescript
(value: any) => boolean
```
Checks if a value is valid.

### utils.isObject(value)
```typescript
(value: any) => boolean
```
Checks if a value is an object.

### utils.isFunction(value)
```typescript
(value: any) => boolean
```
Checks if a value is a method.

### utils.isBoolean(value)
```typescript
(value: any) => boolean
```
Checks if a value is a bool value.

### utils.formatValue(value, key, defaultValue)
```typescript
(data: any, key: string, defaultValue?: any) => any
```
Get the corresponding value from a certain value, support nesting, such as `const o = { a: { b: { c: 1 } } }`, `formatValue(o, 'a.b.c')` takes the value of `c` .

### utils.formatPrecision(value)
```typescript
(value: string | number, precision?: number) => string
```
Formatting precision.

### utils.formatBigNumber(value)
```typescript
(value: string | number) => string
```
Format large numbers, such as 1000 into 1k, 1000000 into 1M, etc.

### utils.formatDate(dateTimeFormat, timestamp, format)
```typescript
(dateTimeFormat: Intl.DateTimeFormat, timestamp: number, format: string) => string
```
Format date. `format`, such as 'YYYY-MM-DD HH:mm:ss'.

### utils.getLinearSlopeIntercept(coordinate1, coordinate2)
```typescript
(coordinate1: { x: number, y: number }, coordinate2: { x: number, y: number }) => []
```
According to two coordinate points, get the slope and constant term of the line composed of points, namely `k` and `b` in `y = kx + b`.

### utils.getLinearYFromCoordinates(coordinate1, coordinate2, targetCoordinate)
```typescript
(
   coordinate1: { x: number, y: number },
   coordinate2: { x: number, y: number },
   targetCoordinate: { x: number, y: number }
) => number
```
Get the y-axis coordinate value of a point on the line formed by two other coordinate points.

### utils.getLinearYFromSlopeIntercept(kb, targetCoordinate)
```typescript
(kb: Array<number>, targetCoordinate: { x: number, y: number }) => number
```
Get the y-coordinate value of a point on the line formed by the slope and the constant term.

### utils.checkCoordinateOnArc(coordinate, arc)
```typescript
(coordinate: { x: number, y: number }, arc: {
   x: number,
   y: number,
   r: number,
   startAngle: number,
   endAngle: number
}) => boolean
```
Check whether a certain coordinate point is on the arc.
- `coordinate` coordinate point information
- `arc` arc parameter
   - `x` the x-axis value of the center of the circle
   - `y` the y-axis value of the center of the circle
   - `r` radius
   - `startAngle` start angle
   - `endAngle` end angle

### utils.checkCoordinateOnCircle(coordinate, circle)
```typescript
(coordinate: { x: number, y: number }, circle: {
   x: number,
   y: number,
   r: number
}) => boolean
```
Checks whether a certain coordinate point is on a circle.
- `coordinate` coordinate point information
- `circle` circle parameter
   - `x` the x-axis value of the center of the circle
   - `y` the y-axis value of the center of the circle
   - `r` radius

### utils.checkCoordinateOnLine(coordinate, line)
```typescript
(coordinate: { x: number, y: number }, line: { coordinates: Array<{ x: number, y: number }> }) => boolean
```
Check if a certain coordinate point is on the line.

### utils.checkCoordinateOnPolygon(coordinate, polygon)
```typescript
(coordinate: { x: number, y: number }, polygon: { coordinates: Array<{ x: number, y: number }> }) => boolean
```
Checks whether a certain coordinate point is on a polygon.

### utils.checkCoordinateOnRect(coordinate, rect)
```typescript
(coordinate: { x: number, y: number }, rect: {
   x: number,
   y: number,
   width: number,
   height: number
}) => boolean
```
Checks whether a certain coordinate point is on a rectangle.
- `coordinate` coordinate point information
- `rect` rectangle parameter
   - `x` starting point x-axis value
   - `y` starting point y-axis value
   - `width` width
   - `height` height

### utils.checkCoordinateOnText(coordinate, text, styles)
```typescript
(
   coordinate: { x: number, y: number },
   text: {
      x: number,
      y: number,
      text: any,
      align?: 'center' | 'end' | 'left' | 'right' | 'start',
      baseline?: 'alphabetic' | 'bottom' | 'hanging' | 'ideographic' | 'middle' | 'top'
   },
   styles: {
      color?: string,
      size?: number,
      family?: string,
      weight?: number | string
   }
) => boolean
```
Check if a certain coordinate point is on the text.
- `coordinate` coordinate point information
- `text` text parameter
   - `x` starting point x-axis value
   - `y` starting point y-axis value
   - `text` text content
   - `align` horizontal alignment
   - `baseline` vertical alignment
- `styles` styles
   - `color` color
   - `size` size
   - `family` font
   - `weight` weight

### utils.drawArc(ctx, arc, styles)
```typescript
(
   ctx: CanvasRenderingContext2D,
   arc: {
      x: number,
      y: number,
      r: number,
      startAngle: number,
      endAngle: number
   },
   styles: {
      style?: 'solid' | 'dashed',
      size?: number,
      color?: string,
      dashedValue?: number[]
   }
) => void
```
Draw an arc.
- `ctx` canvas context
- `arc` arc parameter
   - `x` the x-axis value of the center of the circle
   - `y` the y-axis value of the center of the circle
   - `r` radius
   - `startAngle` starting angle
   - `endAngle` end angle
- `styles` styles
   - `style` arc style
   - `size` thickness
   - `color` color
   - `dashedValue` Dashed parameter value

### utils.drawCircle(ctx, circle, styles)
```typescript
(
   ctx: CanvasRenderingContext2D,
   circle: {
      x: number,
      y: number,
      r: number
   },
   styles: {
      style?: 'stroke' | 'fill' | 'stroke_fill',
      color?: string | CanvasGradient,
      borderColor?: string,
      borderSize?: number,
      borderStyle?: 'solid' | 'dashed',
      borderDashedValue?: Array<number>
   }
) => void
```
Draw the circle.
- `ctx` canvas context
- `circle` circle parameter
   - `x` the x-axis value of the center of the circle
   - `y` the y-axis value of the center of the circle
   - `r` radius
- `styles` styles
   - `style` style
   - `color` color
   - `borderColor` border color
   - `borderSize` border thickness
   - `borderStyle` border style
   - `borderDashedValue` border dashed line parameter value

### utils.drawLine(ctx, line, styles)
```typescript
(
   ctx: CanvasRenderingContext2D,
   line: { coordinates: Array<{ x: number, y: number }> },
   styles: {
      style?: 'solid' | 'dashed',
      size?: number,
      color?: string,
      dashedValue?: number[]
   }
) => void
```
Draw the line.
- `ctx` canvas context
- `line` line parameter
- `styles` styles
   - `style` line style
   - `size` thickness
   - `color` color
   - `dashedValue` Dashed parameter value

### utils.drawPolygon(ctx, polygon, styles)
```typescript
(
   ctx: CanvasRenderingContext2D,
   polygon: { coordinates: Array<{ x: number, y: number }> },
   styles: {
      style?: 'stroke' | 'fill' | 'stroke_fill',
      color?: string | CanvasGradient,
      borderColor?: string,
      borderSize?: number,
      borderStyle?: 'solid' | 'dashed',
      borderDashedValue?: Array<number>
   }
) => void
```
Draw the polygon.
- `ctx` canvas context
- `polygon` polygon parameter
- `styles` styles
   - `style` style
   - `color` color
   - `borderColor` border color
   - `borderSize` border thickness
   - `borderStyle` border style
   - `borderDashedValue` border dashed line parameter value

### utils.drawRect(ctx, rect, styles)
```typescript
(
   ctx: CanvasRenderingContext2D,
   rect: {
      x: number,
      y: number,
      width: number,
      height: number
   },
   styles: {
      style?: 'stroke' | 'fill' | 'stroke_fill',
      color?: string | CanvasGradient,
      borderColor?: string,
      borderSize?: number,
      borderStyle?: 'solid' | 'dashed',
      borderDashedValue?: Array<number>,
      borderRadius?: number
   }
) => void
```
Draw a rectangle.
- `ctx` canvas context
- `rect` rectangle parameter
   - `x` starting point x-axis value
   - `y` starting point y-axis value
   - `width` width
   - `height` height
- `styles` styles
   - `style` style
   - `color` color
   - `borderColor` border color
   - `borderSize` border thickness
   - `borderStyle` border style
   - `borderDashedValue` border dashed line parameter value
   - `borderRadius` border radius


### utils.drawText(ctx, text, styles)
```typescript
(
   ctx: CanvasRenderingContext2D,
   text: {
      x: number,
      y: number,
      text: any,
      align?: 'center' | 'end' | 'left' | 'right' | 'start',
      baseline?: 'alphabetic' | 'bottom' | 'hanging' | 'ideographic' | 'middle' | 'top'
   },
   styles: {
      color?: string,
      size?: number,
      family?: string,
      weight?: number | string
   }
) => void
```
Draw text.
- `ctx` canvas context
- `text` text parameter
   - `x` starting point x-axis value
   - `y` starting point y-axis value
   - `text` text content
   - `align` horizontal alignment
   - `baseline` vertical alignment
- `styles` styles
   - `color` color
   - `size` size
   - `family` font
   - `weight` weight
  
### utils.drawRectText(ctx, rectText, styles)
```typescript
(
   ctx: CanvasRenderingContext2D,
   rectText: {
      x: number,
      y: number,
      text: any,
      align?: 'center' | 'end' | 'left' | 'right' | 'start',
      baseline?: 'alphabetic' | 'bottom' | 'hanging' | 'ideographic' | 'middle' | 'top'
   },
   styles: {
      style?: 'stroke' | 'fill' | 'stroke_fill',
      color?: string,
      size?: number,
      family?: string,
      weight?: number | string,
      paddingLeft?: number,
      paddingTop?: number,
      paddingRight?: number,
      paddingBottom?: number,
      borderStyle?: 'solid' | 'dashed',
      borderDashedValue?: number[],
      borderSize?: number,
      borderColor?: string,
      borderRadius?: number,
      backgroundColor?: string
   }
) => void
```
Draw text with background.
- `ctx` canvas context
- `rectText` text parameter
   - `x` starting point x-axis value
   - `y` starting point y-axis value
   - `text` text content
   - `align` horizontal alignment
   - `baseline` vertical alignment
- `styles` styles
   - `style` style
   - `color` color
   - `size` size
   - `family` font
   - `weight` weight
   - `paddingLeft` left padding,
   - `paddingTop` top padding,
   - `paddingRight` right padding,
   - `paddingBottom` bottom padding,
   - `borderColor` border color
   - `borderSize` border thickness
   - `borderStyle` border style
   - `borderRadius` border radius size
   - `borderDashedValue` border dashed line parameter value
   - `backgroundColor` background color