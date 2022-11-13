const CHANNEL_POST_URL = "Add Discord webhook id"; 
const CALENDAR_ID = "Add Calendar ID";
const NO_VALUE_FOUND = "N/A";

eval(UrlFetchApp.fetch('https://cdn.jsdelivr.net/npm/luxon@2.0.2/build/global/luxon.min.js').getContentText());
let DateTime = luxon.DateTime;
const DTnow = DateTime.now().startOf('minute');

function postEventsToChannel() {
  let optionalArgs = {
    timeMin: DTnow.toISO(),
    showDeleted: false,
    singleEvents: true,
    orderBy: 'updated'
  };
  let response = Calendar.Events.list(CALENDAR_ID, optionalArgs);
  let events = response.items;
  if (events.length > 0) {
    
      let event = events[events.length - 1];
      let ISOStartDate = event.start.dateTime || event.start.date;
      let ISOEndDate = event.end.dateTime || event.end.date;

      // Build the POST request
      let options = {
          "method": "post",
          "headers": {
              "Content-Type": "application/json",
          },
          "payload": JSON.stringify({
              "content": "‌",
              "embeds": [{
              "author": {
                  "name": `${event.summary}`,
                  "icon_url": "https://cdn.discordapp.com/attachments/696400605908041794/888874282950750238/1200px-Google_Calendar_icon_28202029.png"
              },
                "timestamp": DTnow.toISO(),
                "description":`[Google Event Link](${event.htmlLink})`,
                "color": 1425196,
                "fields":[
                    {
                      "name":"Start Time",
                      "value": ISOToDiscordUnix(ISOStartDate) ?? NO_VALUE_FOUND,
                      "inline":false
                    },
                    {
                      "name":"End Time",
                      "value":ISOToDiscordUnix(ISOEndDate) ?? NO_VALUE_FOUND,
                      "inline":false
                    },
                    {
                      "name":"Location",
                      "value":event.location ?? NO_VALUE_FOUND,
                      "inline":false
                    },
                    {
                      "name":"Description",
                      "value":event.description ?? NO_VALUE_FOUND,
                      "inline":false
                    }
                ]
            }]
          })
      };
      Logger.log(options, null, 2);
      UrlFetchApp.fetch(CHANNEL_POST_URL, options);
    
  } else {
    Logger.log(`No events found.`);
  }
}

function ISOToDiscordUnix(isoString) {
  return `<t:${Math.floor(DateTime.fromISO(isoString).toSeconds())}:F>`
}
