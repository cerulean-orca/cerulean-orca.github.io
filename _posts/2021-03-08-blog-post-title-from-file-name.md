## Stopping Bill Murray Syndrome

For a long time I had the feeling that each week was a new start to my sales journey. I struggled to chain days and weeks together, which made my follow-up's inconsistent. I was like the family cat that disappears only to come back 6 months later, without any acknowledgement that I am weird and mysterious.

The main reason for my Groundhog Day sales journey was that we're a bootstrapped company and our founder is as tough as early Amazon Jeff B. in controlling costs. Combined with the horrible design of most CRM's and our record keeping was spread out across an ever expanding number of Google Sheets.

This week, I decided to change that. My goal is to have a clear view of who I will follow-up with next week from this week. 

---

### Exporting from Google

The Gmail design is always changing slightly, which makes most tutorials on the internet obselete. 

For example, there were a lot of pages suggesting that Gmail had a "download" feature when you clicked on the 3 dots button. That is no more. 

To export from Gmail, we have 2 options. The first is form Takeout, but that only allows us to export ALL our emails.

I only want to export SENT emails from a certain day or 5 day business week. 

is:sent from:(email address) after:2025/month.number/day.number before:2025/month.number/day.number

is the way to go.

file:///Users/keremgunes/Desktop/Screen%20Shot%202025-02-14%20at%2015.21.11.png

#### Some T-SQL Code

```tsql
SELECT This, [Is], A, Code, Block -- Using SSMS style syntax highlighting
    , REVERSE('abc')
FROM dbo.SomeTable s
    CROSS JOIN dbo.OtherTable o;
```

#### Some PowerShell Code

```powershell
Write-Host "This is a powershell Code block";

# There are many other languages you can use, but the style has to be loaded first

ForEach ($thing in $things) {
    Write-Output "It highlights it using the GitHub style"
}
```
