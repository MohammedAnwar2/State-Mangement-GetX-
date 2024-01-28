```dart
// ignore_for_file: non_constant_identifier_names, unnecessary_brace_in_string_interps

import 'package:get/get.dart';
import 'package:intl/intl.dart';
import 'package:schedule_app/core/controller/task/taskDatabaseController.dart';
import 'package:schedule_app/data/models/taskModel.dart';

final controller = Get.find<TaskDatabaseController>();

String convertDateTimeToString_MMMDHMMA(DateTime dateTime) {
  return DateFormat('MMM d h:mm a').format(dateTime);
}

DateTime convertTimeStringToDateTime_MMMD(String timeString) {
  DateFormat format = DateFormat("MMM d h:mm a");
  return format.parse(timeString);
}

DateTime convertTimeStringToDateTime_Edhmma(String timeString) {
  DateFormat format = DateFormat("E h:mm a");
  return format.parse(timeString);
}

DateTime convertTimeStringToDateTime_Edhmm(String timeString) {
  DateFormat format = DateFormat("E HH:mm");
  return format.parse(timeString);
}

DateTime convertTimeStringToDateTime_HMMA(String timeString) {
  DateFormat format = DateFormat("h:mm a");
  return format.parse(timeString);
}

DateTime convertTimeStringToDateTime_HMMA_forTimeTable(String timeString) {
  DateFormat format = DateFormat("E h:mm a");
  return format.parse(timeString);
}

//to let us know if the task is not complete in day or not
bool checkMissingTime({required String dt_ForTask}) {
  if (convertTimeStringToDateTime_MMMD(dt_ForTask).compareTo(
          convertTimeStringToDateTime_MMMD(DateFormat('MMM d h:mm a').format(
              DateTime(DateTime.now().year, DateTime.now().month,
                  DateTime.now().day)))) <
      0) {
    //the missing time
    // if () {
    //   print(
    //       "$dt_ForTask is before ${convertTimeStringToDateTime_MMMD(DateFormat('MMM d, h:mm a').format(DateTime(DateTime.now().year, DateTime.now().month, DateTime.now().day)))}");
    // }

    return false;
  } else if (convertTimeStringToDateTime_MMMD(dt_ForTask).compareTo(
          convertTimeStringToDateTime_MMMD(DateFormat('MMM d h:mm a').format(
              DateTime(DateTime.now().year, DateTime.now().month,
                  DateTime.now().day)))) >
      0) {
    //NOT the missing time
    // if () {
    //   print(
    //       "$dt_ForTask is after ${convertTimeStringToDateTime_MMMD(DateFormat('MMM d, h:mm a').format(DateTime(DateTime.now().year, DateTime.now().month, DateTime.now().day)))}");
    // }

    return true;
  } else {
    //NOT the missing time
    return true;
  }
}

//to let us know if the task is still there in database for two days or not
differenceTimeForMissingTask({required String taskDate, required int id}) {
  // Parse the date strings into DateTime objects
  DateTime getTaskDate = convertTimeStringToDateTime_MMMD(taskDate);
  DateTime todayDate = convertTimeStringToDateTime_MMMD(
      DateFormat('MMM d h:mm a').format(DateTime(
          DateTime.now().year, DateTime.now().month, DateTime.now().day)));
  // if () {
  //   print("${getTaskDate} getTaskDate");
  // }
  // if () {
  //   print("${todayDate} todayDate");
  // }
  // Calculate the difference between the two dates
  Duration difference = todayDate.difference(getTaskDate);

  print("Difference: ${difference.inDays} days");
//delete task after two days
  if (difference.inDays >= 1) {
    controller.deleteMissingTask(id: id);
    return true;
  } else {
    return false;
  }
}

//to let us know if the task is still there in database for one days or not
differenceTimeForContinueTask({required String taskDate, required Task task}) {
  // Parse the date strings into DateTime objects
  DateTime getTaskDate = convertTimeStringToDateTime_MMMD(taskDate);
  DateTime todayDate = convertTimeStringToDateTime_MMMD(
      DateFormat('MMM d h:mm a').format(DateTime(
          DateTime.now().year, DateTime.now().month, DateTime.now().day)));
  // if () {
  //   print("${getTaskDate} getTaskDate");
  // }
  // if () {
  //   print("${todayDate} todayDate");
  // }
  // Calculate the difference between the two dates
  Duration difference = todayDate.difference(getTaskDate);

  //print("Difference: ${difference.inHours} Hours");
//delete task after two days
  if (difference.inDays >= 0 && task.isCompleted == true) {
    controller.deleteMissingTask(id: task.id!);
    return true;
  } else {
    return false;
  }
}

// void main() {
//   // Get the current date
//   DateTime currentDate = DateTime.now();

//   // Generate the list of 30 days
//   List<Map<String, dynamic>> daysList = generateDateOfDayesList(currentDate, 30);

//   // Display each day with its date and day of the week
// //  for (var day in daysList) {
//  //   print('${day['date']} -> ${day['day']}');
// //  }

//     for (var i=0 ;i<daysList.length;i++) {
//     print('${daysList[i]['date']} -> ${daysList[i]['day']}');
//   }
// }

List<Map<String, dynamic>> generateDateOfDayesList(
    DateTime date, int numberOfDays) {
  List<Map<String, dynamic>> result = [];

  // Calculate the middle index
  int middleIndex = (numberOfDays / 2).floor();

  // Add the current day to the middle of the result list
  result.add({'date': date.day, 'day': getFormattedWeekday(date)});

  // Iterate backward from the middle index
  for (int i = 1; i <= middleIndex; i++) {
    DateTime previousDay = date.subtract(Duration(days: i));
    result.insert(
        0, {'date': previousDay.day, 'day': getFormattedWeekday(previousDay)});
  }

  // Iterate forward from the middle index
  for (int i = 1; i <= numberOfDays - middleIndex - 1; i++) {
    DateTime nextDay = date.add(Duration(days: i));
    result.add({'date': nextDay.day, 'day': getFormattedWeekday(nextDay)});
  }

  return result;
}

String getFormattedWeekday(DateTime date) {
  return DateFormat.E().format(date); // Format day of the week (e.g., Mon)
}

String formatTime(int time) {
  if (time >= 60) {
    int hours = time ~/ 60;
    int minutes = time % 60;

    if (hours > 0 && minutes > 0) {
      return '$hours ${hours == 1 ? 'hour' : 'hours'} $minutes ${minutes == 1 ? 'min' : 'mins'}';
    } else if (hours > 0) {
      return '$hours ${hours == 1 ? 'hour' : 'hours'}';
    } else {
      return '$minutes ${minutes == 1 ? 'min' : 'mins'}';
    }
  } else {
    return '$time ${time == 1 ? 'min' : 'mins'}';
  }
}

int convertTimeStringToMinutes(String timeString) {
  // Parse the time string using the specified format
  final DateTime parsedTime = DateFormat("MMM d h:mm a").parse(timeString);

  // Calculate the total minutes
  final int totalMinutes = parsedTime.hour * 60 + parsedTime.minute;

  return totalMinutes;
}

String addTimes(DateTime time1, DateTime time2) {
  final format = DateFormat('h:mm a');
  final duration =
      time1.add(Duration(hours: time2.hour, minutes: time2.minute));

  if (duration.hour > 23) {
    final hours = duration.hour - 24;
    final minutes = duration.minute;
    return format.format(
        DateTime(duration.year, duration.month, duration.day, hours, minutes));
  } else if (duration.minute > 59) {
    final hours = duration.hour;
    final minutes = duration.minute - 60;
    return format.format(
        DateTime(duration.year, duration.month, duration.day, hours, minutes));
  } else {
    return format.format(duration);
  }
}

String addMinutesToTime(
    {required String originalTime, required int minutesToAdd}) {
  // Parse the original time string
  DateTime parsedTime = DateFormat('E h:mm a').parse(originalTime);

  ///?????????????????

  // Add specified minutes to the parsed time
  DateTime newTime = parsedTime.add(Duration(minutes: minutesToAdd + 1));

  // Format the new time as a string

  String formattedNewTime =
      DateFormat('E h:mm a').format(newTime); //?????????????????????

  return formattedNewTime;
}

String convert12To24Hour(String twelveHourTime) {
  DateTime dateTime = DateFormat('h:mm a').parse(twelveHourTime);

  // Check if it's PM, then convert to 24-hour
  if (DateFormat('a').format(dateTime) == 'PM') {
    return DateFormat('HH:mm a').format(dateTime);
  } else {
    // If it's AM, return the same time
    return DateFormat('h:mm a').format(dateTime);
  }
}

String convert24To12Hour(String twentyFourHourTime) {
  DateTime dateTime = DateFormat('HH:mm').parse(twentyFourHourTime);

  // Check if it's PM; if so, convert to 12-hour format, otherwise return as is
  if (dateTime.hour >= 12) {
    return DateFormat('h:mm a').format(dateTime);
  } else {
    return twentyFourHourTime;
  }
}

//----------------------------------------------------------------

String convertTo24HourFormat(String twelveHourTime) {
  DateTime dateTime = DateFormat('E h:mm a').parse(twelveHourTime);
  print("----------   ${DateFormat('E HH:mm').format(dateTime)}  ----------");
  return DateFormat('E HH:mm').format(dateTime);
}

// String convertTo24HourFormat_Ehmm(String twelveHourTime) {
//   DateTime dateTime = DateFormat('E h:mm').parse(twelveHourTime);
//   print("----------   ${DateFormat('E HH:mm').format(dateTime)}  ----------");
//   return DateFormat('E HH:mm').format(dateTime);
// }

String convertTo12HourFormat(String twentyFourHourTime) {
  DateTime dateTime = DateFormat('E HH:mm').parse(twentyFourHourTime);
  return DateFormat('E h:mm a').format(dateTime);
}

String convertTo24HourFormat_forTimeBlocks(String inputTime) {
  // Define input and output date formats
  final inputFormat = DateFormat("hh:mm a");
  final outputFormat = DateFormat("HH:mm");

  // Parse the input time string
  final parsedTime = inputFormat.parse(inputTime);

  // Format the time in 24-hour format
  final outputTime = outputFormat.format(parsedTime);

  return outputTime;
}

```
