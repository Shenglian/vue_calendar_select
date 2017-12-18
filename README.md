# vue_calendar_select

> select start_date and end_date in datepicker

## Install

``` bash
$ npm install vue_calendar_select --save
```
## Usage

*setDays* prop if passed should be a [YYYY/MM/DD, YYYY/MM/DD]
``` html
<script>
  const setDays = ['2017/01/01', '2017/12/31'];
</script>
<CalendarSelect :setDays="setDays"></CalendarSelect>
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
| setDays                 | Array           | '2001/3/20', '2018/3/20'  | array[0] = startDate, array[1] = endDate           |
| datepickerStatus        | String          |                           | set 'disable' prop for disable datepicker          |

## Events

These events are emitted on actions in the datepicker

| Event             | Output            | Description                          |
|-------------------|-------------------|--------------------------------------|
| sendDate          | startDay, endDay  | get startDay and endDay value        |

## Log

#### 2017/12/18
1. remove `placeholderTextStartDay`, `placeholderTextEndDay` props.
2. add `datepickerStatus` prop.
3. It world change datepicker by year on double arrow button.

