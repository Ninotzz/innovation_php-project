# innovation

## Innovation & Technology Portal

This project is a dynamic, database-driven web application built with PHP and MySQL. It functions as a small-scale Content Management System (CMS) focused on sharing articles and information about technology and innovation. The application features role-based user management, a secure authentication system, and a dedicated admin panel for managing users and content.

---

### About The Project

This web portal was developed to create a space where registered users can read articles on Innovation, Communication, and Technology (ICT). The site features a public-facing side with article previews and a private, members-only area where logged-in users can view full content.

Administrators have extended privileges through a backend panel, allowing them to perform CRUD (Create, Read, Update, Delete) operations on users and articles, making content management simple and centralized.

### Key Features

    Secure User Authentication: Complete login, logout, and session management functionality.

    Role-Based Access Control:

        Admin: Full access to the admin panel, user management, and content management.

        User: Can register, log in, and view the full content of articles.

    Admin Control Panel: A dedicated backend for administrators to:

        View all registered users.

        Grant or revoke admin privileges.

        Delete users (with a safeguard to prevent self-deletion).

    Content Management:

        Admins can create and publish new articles.

        Admins can add and list new software/tools.

        Admins can delete existing articles directly from the article page.

    Dynamic Content Rendering: The application dynamically serves content based on user's authentication status (e.g., article previews for guests, full articles for logged-in users).

    Database Initialization: A simple migration script (boot.php) creates the necessary database tables and populates them with a default admin user, streamlining the initial setup.

#### Built With

- PHP

- MySQL

- HTML5

- CSS3

---

### Getting Started

To get a local copy up and running, follow these simple steps.

#### Prerequisites

You will need a local server environment with PHP and MySQL. The most common stacks are:

- XAMPP (for Windows, macOS, Linux)

- MAMP (for macOS)

- WAMP (for Windows)

### Installation Guide ⚙️

Clone the repository into your local server's web root directory (e.g., htdocs in XAMPP).
    Bash

    git clone https://github.com/your_username/your_repository.git

#### Set up the database connection:

In the project folder, find the file database.example.php.

Make a copy of this file and rename the copy to database.php.

Open database.php and fill in your local MySQL database credentials (hostname, username, password, and database name).

#### Create the database:

Using a tool like phpMyAdmin, create a new, empty database.

Ensure the name you choose matches the database name you entered in database.php.

#### Run the initial setup script:

Open your web browser and navigate to the boot.php script in your project directory. This will set up the essential utenti table and create a default administrator account.

    Go to: http://localhost/your_project_folder/boot.php

You should see a success message on the screen. For security, it's a good practice to delete or rename boot.php after this step is complete.

#### Log in as Admin:

Navigate to the login page.

Log in with the default admin credentials:

Email: root@info.it 

Password: admin 

#### Run remaining database migrations:

After logging in as the administrator, navigate to the migrations.php page in your browser.

    Go to: http://localhost/your_project_folder/migrations.php

You will see a checklist of available database scripts. Select the checkboxes for 

    create_articoli_table, create_tools_table, populate_articoli_table, populate_utenti_table, and populate_tools_table to create the rest of the tables and fill them with sample data.

Click the "Conferma" button to run them.

All set! The application is now fully installed and configured. You can start exploring its features.

### Usage

#### Admin Access

Navigate to the login page: http://localhost/innovation/login.php.

Log in using the default admin credentials that were created by the sql/populate_utenti_table_admin.sql script.

Upon successful login, you will be redirected to the Admin User Management panel (/admin.php).

#### Regular User

Navigate to the (un-provided) register.php page to create a new account.

Log in via the login.php page.

You will be redirected to the homepage, where you can now browse and read the full versions of all articles.

---

### Code Snippets Showcase

Here are a few examples of the code logic from the project.

Admin Panel - User Management (admin.php)
This snippet shows how the list of users is displayed and how forms are generated to allow admins to change user roles or delete users.
PHP

    /* Stampo tutti gli utenti registrati */
    $sql = 'SELECT * FROM utenti';
    $result = mysqli_query($conn, $sql);
    
    while ($row = mysqli_fetch_assoc($result)) {
        // ... extract user data ...
        echo '
              <tr>
                  <td>' . $id . '</td>
                  <td>' . $email . '</td>
                  // ... other cells ...
                  <td>
                      <form action="admin.php" method="POST">
                           <input type="hidden" name="id" value="' . $id . '">
                           <input type="hidden" name="admin" value="' . $admin . '">
                           <button type="submit" name="change_rule">
                               <i class="far fa-edit"></i>
                           </button>
                      </form>
                  </td>
                  // ... delete form ...
              </tr>';
    }

Article Page - Conditional Content (articolo.php)
This shows how the application displays a short preview to guests and the full article content only to logged-in users.
PHP

    /* Se la sessione è loggata allora mostro tutto il corpo e, se presente, il link, sennò solo la preview*/
    if ( isset($_SESSION['logged']) && $_SESSION['logged'] == true) {
        if ($gdrive) {
            echo "<div class='divtesto'>
                      <a target='_blank' href='$gdrive'>Link utile: $gdrive</a>
                 </div>";
            }
        echo "<div class='testo'> $corpo </div>";
        }
    else {
        echo" <div class='testo'>  $preview  </div>
              <div class='testo'>
                  <p>Effettua il <a href='/innovation/login.php'>login</a> per visualizzare l'articolo completo</p>
              </div>";
        }
