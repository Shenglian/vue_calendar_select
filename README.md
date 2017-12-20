# vue_calendar_select

> select start_date and end_date in datepicker

## Install

``` bash
$ npm install vue_calendar_select --save
```
## Usage

*setLimitDays* prop if passed should be a [YYYY/MM/DD, YYYY/MM/DD]
``` html
<script>
  const setLimitDays = ['2017/01/01', '2017/12/31'];
</script>
<CalendarSelect :setLimitDays="setLimitDays"></CalendarSelect>
```

*setCurrentDays* prop if passed should be a [YYYY/MM/DD, YYYY/MM/DD]
``` html
<script>
  const setCurrentDays = ['2017/01/01', '2017/12/31'];
</script>
<CalendarSelect :setCurrentDays="setCurrentDays"></CalendarSelect>
```

*datepickerStatus* prop if pass 'disable', datepicker will disable and get to disable class style
``` html
<script>
  const datepickerStatus = 'disable';
</script>
<CalendarSelect :datepickerStatus="datepickerStatus"></CalendarSelect>
```

Emits events, receive startDate and endDate value from inside
``` html
<CalendarSelect @sendDate="receiveDate"></CalendarSelect>
<script>
  function receiveDate({startDay, endDay} = {}){
    console.log(`startDay: ${startDay}`);
    console.log(`endDay: ${endDay}`);
  }
</script>
```

## Available props

| Prop                    | Type            | Default                   | Description                                        |
|-------------------------|-----------------|---------------------------|----------------------------------------------------|
| setLimitDays            | Array           | '2017/3/20', '2018/3/20'  | set limit start day and end day                    |
| setCurrentDays          | Array           | '', ''                    | set current start day and end day                  |
| datepickerStatus        | String          | ''                        | set 'disable' prop for disable datepicker          |

## Events

These events are emitted on actions in the datepicker

| Event             | Output            | Description                                  |
|-------------------|-------------------|----------------------------------------------|
| sendDate          | startDay, endDay  | get current startDay and endDay value        |

## Log

#### 2017/12/18
1. remove `placeholderTextStartDay`, `placeholderTextEndDay` props.
2. add `datepickerStatus` prop.
3. It world change datepicker by year on double arrow button.

#### 2017/12/20
1. change prop `setDays` to `setLimitDays`
2. add `setCurrentDays` prop.

