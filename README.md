# innovation

## Innovation & Technology Portal

This project is a dynamic, database-driven web application built with PHP and MySQL. It functions as a small-scale Content Management System (CMS) focused on sharing articles and information about technology and innovation. The application features role-based user management, a secure authentication system, and a dedicated admin panel for managing users and content.

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

### Built With

- PHP

- MySQL

- HTML5

- CSS3

### Getting Started

To get a local copy up and running, follow these simple steps.

#### Prerequisites

You will need a local server environment with PHP and MySQL. The most common stacks are:

- XAMPP (for Windows, macOS, Linux)

- MAMP (for macOS)

- WAMP (for Windows)

### Installation

Clone the repository into your local server's root directory (e.g., htdocs in XAMPP).
    Bash

    git clone https://github.com/your_username/your_repository.git

Navigate to the project directory. The path will likely be htdocs/innovation.

Create a MySQL database for the project using a tool like phpMyAdmin. Let's name it innovation_db.

Configure the database connection. Open the config/database.php file and update the connection variables with your database credentials.
PHP

    <?php
    $servername = "localhost";
    $username = "root"; // Or your DB username
    $password = ""; // Or your DB password
    $dbname = "innovation_db"; // The database you created

    // Create connection
    $conn = mysqli_connect($servername, $username, $password, $dbname);

    // Check connection
    if (!$conn) {
        die("Connection failed: " . mysqli_connect_error());
    }
    ?>

Run the database migration script. Open your web browser and navigate to the boot.php file to initialize the database tables and create the default admin user.

    http://localhost/innovation/boot.php

After running this script, you should see success messages for the table migrations. For security, you may want to delete or rename boot.php after the initial setup.

You're all set! Navigate to the project's homepage.

    http://localhost/innovation/index.php

### Usage

#### Admin Access

Navigate to the login page: http://localhost/innovation/login.php.

Log in using the default admin credentials that were created by the sql/populate_utenti_table_admin.sql script.

Upon successful login, you will be redirected to the Admin User Management panel (/admin.php).

#### Regular User

Navigate to the (un-provided) register.php page to create a new account.

Log in via the login.php page.

You will be redirected to the homepage, where you can now browse and read the full versions of all articles.

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
