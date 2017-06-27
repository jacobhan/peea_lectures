## Synopsis

Hello, and this is the 500 Startups Financial Data website. I built the structure around it so hopefully it will be relatively straightforward to set up this site. It probably shouldn't take more than a few hours, no less than a day.

## How It Works

Spreadsheet data is pulled from the **Google Sheets/Drive API**, and that information is stored in the controllers. This prevents the need for the setup of an actual server. 

Dynos are automatically updated (I believe once a day), but to see immediate changes on the website after making changes to the spreadsheet, you must reboot the server. I deployed on Heroku so I use PostgreSQL, but if you're planning on using something else then you can just change it to SQLite3, it doesn't really matter.

## API Reference

I use the Google Sheets/Drive API. Familiarty with this API is not needed as I pretty much did everything already, but for clearer documentation than what's on Google, the documentation I found most helpful is at the Github link below.

* [Google Drive Ruby Library](https://github.com/gimite/google-drive-ruby)

## What To Change

You must change the spreadsheet key that is in your API file. The spreadsheet key is found in the URL of your spreadsheet. You must also make sure that your Google account has access to this spreadsheet. For user authentication, you may need to go to your developer console.

I understand that all spreadsheets are not made in the same way, so I made it simple so that all of the column numbers just have to be replaced in the 'quickstart.rb' file. If your spreadsheet doesn't follow the temp file that is attached in this README, then you may have to change the numbers. If you also want to change the way that the calculations are done or data is stored, then simply change the if statement in the giant for loop for the respective column.

```
// For the spreadsheet in 500 Startups Korea, the 15th column corresponds to the Current Valuation (USD Adjusted). 
All you should do is replace the 15's with wherever your corresponding information is.

if (!(row[15].blank?))
	asdf = row[15]
	asdf[0] = "0"
	asdf = asdf.tr(',', '')
	asdf = asdf.to_i

	round.push(asdf)
end
```

I use global data structures to store everything, which is kind of inefficient but gets the job done.

## How The Information Is Parsed

All of the spreadsheet data is read as strings. 

For currency, I simply remove the first character and all of the commas, then turn them into numbers. After I made all the calculations necessary for the graphs, I convert these numbers back to strings before embedding them into the HTML.

## How The Information Is Stored

Nearly all of the information about the companies is stored into a giant list (I believe it is a four dimensional array). Each element at the base level is a company, and each company has corresponding information and a hash table that contains each round. More detailed documentation about this mega-list is in the 'quickstart.rb' file.

## Update Columns

Due to the lack of functional triggers for Google Sheets, I made extra columns where you input every time a company's PPS or valuation gets updated (they're at the far right of the spreadsheet). I pull this information as well using the API, but store them separately in arrays. Inside of the javascript section of the HTML files, I treat every "update" as a mini-round. As a result, the graphs that are shown include all of these updates as well for a more comprehensive visualization.

## Tips

As long as your spreadsheet is in the same relative format as the temp file, then it should be compatible as long as you replace the keys as well as your account information. You may need to authenticate once from the command line, but afterwards you'll never need to again. Change column numbers as necessary, but other than that, there shouldn't need to be any changes made to any other files.
