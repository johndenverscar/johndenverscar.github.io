---
layout: post
title: Database Users and Roles
date: 2019-04-12 12:00
---

Database users and roles, in short, for the rest of us.

I'm a huge database nerd.
I love the tech, I love the inner workings, I love the administration, and I love working with them.
I've noticed a recent trend with database tech becoming increasingly accessible.
People are starting to manage databases without knowing what it entails.
That's alright, but ORM's aren't database admins, they're training wheels.
When you lean on them too much, you'll find yourself falling over a lot when it's not there to hold your hand.

Now go [install MySQL][0] and follow allong.

### Changing the root password

FYI, my prompt looks like this `user ~`. Anything after the `~` is stuff for you to type.

Now that you have MySQL installed, we're going to set the root user's password.

```
user ~ mysqladmin -u root password "new_password";
```

Keep this password in mind, it's going to be important.
Write it down if you have to but don't give it out.
The root user can do whatever they want, including destroying your DBs.

### Creating a database

Now we're going to create a schema and a user on that schema.

First the schema, otherwise known as a database.

```
user ~ mysql -u root -p
Enter Password: <type it in, it wont show up but it's reading your input>
*** Copywrite crap ***
mysql> create database tutorial;
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| test               |
| tutorial           | # this is the important one
+--------------------+
5 rows in set (0.00 sec)

mysql> use tutorial;
Database changed
mysql> select database() from dual;
+------------+
| database() |
+------------+
| tutorial   |
+------------+
1 row in set (0.00 sec)

```

Congratulations, you now have a database.

### Create a user

Unfortunately, the only one who can use it is the root user.
That's no bueno.
We're going to fix that now.

```
mysql> # first let's list the users
mysql> select User from mysql.users;
+---------------+
| User          |
+---------------+
| mysql.session | # system user
| mysql.sys     | # system user, don't mind those
| root          | # Us
+---------------+
3 rows in set (0.00 sec)
mysql> # Now we'll make our own to use the tutorial DB
mysql> create user 'newuser'@'localhost' identified by 'password';
Query OK, 0 rows affected (0.00 sec)

mysql> select User from mysql.users;
+---------------+
| User          |
+---------------+
| mysql.session | # system user
| mysql.sys     | # system user, don't mind those
| root          | # Us
| newuser       | # Our new user!
+---------------+
4 rows in set (0.00 sec)
```

Yeah!
We have a new user!
That can't do anything...
Yet.

# Adding permissions

We want this user to be able to do anything on __just__ the tutorial DB.

```
mysql> # let's see what they can do
mysql> show grants for newuser@localhost;
+---------------------------------------------+
| Grants for newuser@localhost                |
+---------------------------------------------+
| GRANT USAGE ON *.* TO 'newuser'@'localhost' | # this means they can't do anything
+---------------------------------------------+
1 row in set (0.00 sec)

mysql> grant all privileges on tutorial.* to newuser@localhost;
Query OK, 0 rows affected (0.00 sec)

mysql> show grants for newuser@localhost;
+---------------------------------------------------------------+
| Grants for newuser@localhost                                  |
+---------------------------------------------------------------+
| GRANT USAGE ON *.* TO 'newuser'@'localhost'                   |
| GRANT ALL PRIVILEGES ON `tutorial`.* TO 'newuser'@'localhost' | # we can now work with the tutorial DB
+---------------------------------------------------------------+
2 rows in set (0.00 sec)

```

### Review

That was a bit involved but what we did can be summed up with;

1. Log in as root
    - `mysql -u root -p`
2. Create a DB schema
    - `create database <db name>`
3. Create a new user with no permissions
    - `create user ...`
4. Give the new user permission to use a specific DB
    - `grant all privileges on <db name>.* ...`

I know databases aren't pretty and flashy, but at least learn the basics of admin.
If you do know them, you wont spend hours or days trying to figure out something as simple as what `ER_NO_SUCH_USER` means...

-----
[0]: https://dev.mysql.com/downloads/installer/
