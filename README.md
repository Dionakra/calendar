# School Calendar
This project aims to auto-generate an interactive school calendar where your students (and teachers!) can check which classes are coming!

Functionalities:
* [x] Show / Hide subjects on the calendar
* [x] Save the user's preferences so only selected subjects will show.
* [x] Auto-generate the full calendar only showing the months between the earliest and latest date.
* [x] Show a distinctive pulsating ring whenever an exam occurs.
* [x] Show the room where a certain class happens.
* [x] Color code subjects with something in common (e.g. year for multi-year calendars)
* [x] Highlights the current day for easier lookup.
* [x] Static site generation to be deployed anywhere with no extra cost!

## Prerequisites
* NodeJS LTS

## Installation
```sh
git clone https://github.com/Dionakra/calendar.git
cd calendar
npm install
```

## Configuration
### Updating the calendar
You need to update the [data/calendar.ts](./src/data/calendar.ts) file and modify the calendar. It is done in a TypeScript file to ensure that the data types are correct.
### Change the year
As this repo was created in 2023, the default course date is 2023/24. You can change that in the [routes/+page.svelte](./src/routes/+page.svelte) file.

### Add your custom subjects and classes
Take a look at the [calendar.ts](./src/data/calendar.ts) file, where you can see the current config, where there is one entry per subject, some data and its classes and exams, alongside holidays and special days.

### Add custom colors for your path
In the [paths.ts](./src/data/paths.ts) file you can see that, for each `path` defined in the `calendar.ts` file, there is an entry where you can define the name of the path and the color to be shown

## Deployment
Just run `npm run build` and deploy the `./build` folder to your HTTP Server!