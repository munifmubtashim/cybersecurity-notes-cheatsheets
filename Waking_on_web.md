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



