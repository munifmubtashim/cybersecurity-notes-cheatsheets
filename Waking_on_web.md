[Room](https://tryhackme.com/room/walkinganapplication)
## Exploring The Website



| Feature                 | URL                          | Summary                                                                                                                 |
|------------------------:|-----------------------------:|:------------------------------------------------------------------------------------------------------------------------|
| **Home Page**           | `/`                          | Summary of what Acme IT Support does with a company photo of their staff.                                               |
| **Latest News**         | `/news`                      | List of recently published news articles. Each article links with an id, e.g. `/news/article?id=1`.                    |
| **News Article**        | `/news/article?id=1`         | Displays the individual news article. Some articles are blocked / reserved for premium customers.                       |
| **Contact Page**        | `/contact`                   | Contact form with `name`, `email`, and `message` inputs and a send button.                                               |
| **Customers**           | `/customers`                 | Redirects to `/customers/login`.                                                                                         |
| **Customer Login**      | `/customers/login`           | Login form with `username` and `password` fields.                                                                        |
| **Customer Signup**     | `/customers/signup`          | Signup form with `username`, `email`, `password`, and `password confirmation` fields.                                   |
| **Customer Reset Password** | `/customers/reset`       | Password reset form with an `email` input field.                                                                         |
| **Customer Dashboard**  | `/customers`                 | User's ticket list and a **Create Ticket** button (visible after login).                                                 |
| **Create Ticket**       | `/customers/ticket/new`      | Form to create a ticket: textbox for issue description and file upload option.                                           |
| **Customer Account**    | `/customers/account`         | Edit profile page — change `username`, `email`, and `password`.                                                          |
| **Customer Logout**     | `/customers/logout`          | Logs the user out of the customer area.                                                                                  |


## Developer Tools - Inspector



- Open Developer Tools (right-click → Inspect, or F12 / Ctrl+Shift+I).

- Inspector (Elements): find the element (e.g. div.premium-customer-blocker), edit its CSS — change display: block → display: none (or remove the node). The blocked content appears — changes are local only; refresh restores the site.

- Debugger (Sources): set breakpoints in JavaScript, step through code, inspect variables — useful to see how paywalls are enforced or when content is fetched.

- Network: watch requests, look for XHR/fetch that loads article data, replay or open response to inspect content; you can also disable cache or throttle to reproduce issues.

- Quick reminders: these edits are for debugging/learning only; do not use to bypass paid content unlawfully. DevTools are read/write only in your browser — nothing on the server changes.


## Developer Tools - Debugger


- Open Developer Tools → Debugger (Chrome: Sources, Firefox/Safari: Debugger).

- In the left resources tree open the assets folder and select flash.min.js.

- Click the { } Pretty Print button to reformat the minified file.

- Find the line containing flash['remove'](); and click its line number to set a breakpoint.

- Refresh the page — execution will pause at that breakpoint, the red flash stays visible and you can inspect its DOM and contents (the flag).

- Use the debugger controls to step, inspect variables, or resume execution.

- Reminder: this is a local, temporary debugging technique for learning/pen-testing — do not use it to access content unlawfully.


## Developer Tools - Network


- Open Developer Tools → Network (or F12 / Ctrl+Shift+I → Network).


- Check Preserve log if you want entries to survive a navigation and click the trash can to clear previous logs.


- (Optional) enable Disable cache to force fresh requests.


- Reload the contact page to capture all requests.


- Fill the contact form and click Send Message.


- In the Network list look for the new entry — filter by XHR / Fetch to narrow it down.


- Click that entry and inspect the panels:


- Headers — shows the Request URL, Method (POST/GET) and status.


- Payload / Form Data — shows what the browser sent.


- Response (or Preview) — shows what the server returned; the flag is usually visible here.




- You can right-click the request → Open in new tab to view the raw response, or copy the Response as needed.


- Quick tip: if the response is JSON, use the Preview tab or pretty-print it for readability.


- Reminder: this inspects traffic in your browser only — don’t use it to access content unlawfully.


