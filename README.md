# GoREST | RESTful API Starter kit

<img align="right" width="350px" src="https://cdn.pilinux.me/images/GoREST/logo/GoREST-Logo.png">

[![Go Report Card](https://goreportcard.com/badge/github.com/piLinux/GoREST)][![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2FpiLinux%2FGoREST.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2FpiLinux%2FGoREST?ref=badge_shield)
[01]

GoREST is a starter kit, written in [Golang][11] with [Gin framework][12], for rapid prototyping and developing a
RESTful API. The source code is released under the [MIT license][13] and is free for any personal or commercial
project.



## Database Support

GoREST uses [GORM][21] as its ORM. GORM supports **SQLite3**, **MySQL** and **PostgreSQL**.

In GoREST, **MySQL** driver is included. **SQLite3** and **PostgreSQL** drivers will be included before releasing
the first version of GoREST.



## Demo

For demonstration, a test instance can be accessed [here][31] from a web browser. For API development, it is
recommended to use [Postman][32] or any other similar tool.

Endpoints:
- https://goapi.pilinux.me/api/v1/users
- https://goapi.pilinux.me/api/v1/users/:id
- https://goapi.pilinux.me/api/v1/posts
- https://goapi.pilinux.me/api/v1/posts/:id

To prevent abuse, only HTTP GET requests will be accepted by the demo server.

<img width="650px" src="https://cdn.pilinux.me/images/GoREST/screenshot/GoREST-goapiGET.PNG">



## Setup and start the first production-ready app

- Install a relational database (at the moment, only MySQL driver is included in GoREST)
- Setup an environment to compile the Go codes (a [quick tutorial][41] for any Debian based OS)
- Install `git`
- Download dependencies:
```
go get github.com/gin-gonic/gin
go get github.com/jinzhu/gorm
go get github.com/joho/godotenv
```
- Clone this repository into `$GOPATH/src/github.com/piLinux` directory
```
cd $GOPATH/src/github.com
mkdir piLinux
cd piLinux
git clone https://github.com/piLinux/GoREST.git
```
- Edit `.env.sample` file and save it as `.env` file at the root of your project `$GOPATH/src/github.com/piLinux/GoREST`
- Also edit the `.env.sample` file located at `$GOPATH/src/github.com/piLinux/GoREST/database/migrate` and save it as `.env`
- Inside `$GOPATH/src/github.com/piLinux/GoREST/database/migrate`, run
```
go run autoMigrate.go
```
- Now, from `$GOPATH/src/github.com/piLinux/GoREST`, execute
```
go build main.go
./main.go
```

To the following endpoints GET, POST, PUT and DELETE requests can be sent:
- http://localhost:port/api/v1/users
- http://localhost:port/api/v1/users/:id
- http://localhost:port/api/v1/posts
- http://localhost:port/api/v1/posts/:id


### GET query to read all the data from the database

- http://localhost:port/api/v1/users (get all users and posts belong to each one of them)
- http://localhost:port/api/v1/users/:id (get one specific user and posts belong to him)
- http://localhost:port/api/v1/posts (get all posts)
- http://localhost:port/api/v1/posts/:id (get one specific post)

Received JSON structure for users:

```
{
    "ID": "value",
    "CreatedAt": "date",
    "UpdatedAt": "date",
    "DeletedAt": null,
    "Name": "name",
    "Email": "something@example.com",
    "Posts": [
        {
            "ID": "value",
            "CreatedAt": "date",
            "UpdatedAt": "date",
            "DeletedAt": null,
            "Title": "title",
            "Body": "message body"
        }
	]
}
```

Received JSON structure for posts:

```
{
    "ID": "value",
    "CreatedAt": "date",
    "UpdatedAt": "date",
    "DeletedAt": null,
    "Title": "title",
    "Body": "message body"
}
```

### POST query to create new data

- http://localhost:port/api/v1/users (create new user with no post/one post/many posts)
- http://localhost:port/api/v1/posts (create a new post without any owner)

JSON structure to create a new user with no post:

```
{
    "Name": "name",
    "Email": "something@example.com",
}
```

JSON structure to create a new user with one post:

```
{
    "Name": "name",
    "Email": "something@example.com",
    "Posts": [
        {
            "Title": "title",
            "Body": "message body"
        }
	]
}
```

JSON structure to create a new user with many posts:

```
{
    "Name": "name",
    "Email": "something@example.com",
    "Posts": [
        {
            "Title": "title",
            "Body": "message body"
        },
        {
            "Title": "title",
            "Body": "message body"
        },
        {
            "Title": "title",
            "Body": "message body"
        }
	]
}
```

JSON structure to create a new post without any user:

```
{
    "Title": "title",
    "Body": "message body"
}
```


### PUT query to update any previously saved records

- http://localhost:port/api/v1/users/:id (edit one specific user and posts belong to him)
- http://localhost:port/api/v1/posts/:id (edit one specific post)


### DELETE query to delete an entry

- http://localhost:port/api/v1/users/:id (delete one specific user and all posts belong to him)
- http://localhost:port/api/v1/posts/:id (delete one specific post)



## Features
- GoREST uses [Gin][12] as the main framework, [GORM][21] as the ORM and [GoDotEnv][51] for
environment configuration
- All codes are written and organized following a straightforward and easy-to-understand approach
- For **Logger** and **Recovery**, Gin's in-built middlewares are used
```
router := gin.Default()
```
- Cross Origin Resource Sharing (CORS) middleware is located at **lib/middleware**
```
router.Use(middleware.CORS())
```
- A `many to many` relationship model is included in this project. In future, more sample models
will be published to ease the development phase for any novice developer.



## Missing feature
- It has already been planned to implement JWT authentication. At the moment, GoREST has no
authentication system.



## Architecture

### List of files

```
GoREST
│---README.md
│---LICENSE   
│---.gitignore
│---.env.sample
│---main.go
│
└───config
│	└---configMain.go
│	└---database.go
│	└---server.go
│
│───controller
│	└---post.go
│	└---user.go
│
└───database
│	│---dbConnect.go
│	│
│	└───migrate
│	│	└---autoMigrate.go
│	│	└---.env.sample
│	│
│	└───model
│		└---post.go
│		└---user.go
│		└---userPost.go
│
└───lib
	└───middleware
		└---cors.go
```

For API development, one needs to focus only on the following files and directories:

```
GoREST
│---main.go
│
│───controller
│	└---post.go
│	└---user.go
│
└───database
│	│
│	└───migrate
│	│	└---autoMigrate.go
│	│
│	└───model
│		└---post.go
│		└---user.go
│		└---userPost.go
│
└───lib
	└───middleware
```

### Step 1
- `model`: This package contains all necessary models. Each file is responsible for one specific
table in the database. To add new tables and to create new relations in between those tables,
create new models and place them in this directory. All newly created files should have the same
package name.

### Step 2
- `controller`: This package contains all functions to process all related incoming HTTP requests.

### Step 3
- `autoMigrate.go`: Names of all newly added models should first be included in this file to
automatically create the complete database. It also contains function to delete the previous data
and tables. When only newly created tables or columns need to be migrated, first disable
`db.DropTableIfExists()` function before executing the file.

### Step 4
- `middleware`: All middlewares should belong to this package.

### Step 5 (final step)
- Create new routes inside
```
v1 := router.Group()
{
	...
	...
}
```



## Contributing

- Issues:
	- Use the search tool before opening a new issue
	- If you find any bug, please provide the source code, committed SHA and the steps to
	generate the bug
	- Review existing issues and provide feedback or react to them

- Pull requests
	- Open your pull request against the `master` branch
	- One commit for each unique task (could be one file or a group of files in the same commit)
	- Please only commit tested and validated codes and write proper documentation on how to test
	your proposed codes (until any CI/CD is implemented)
	- If your pull request contains a new feature, please document it in the `README.md` file



## License

© Mahir Hasan - 2019

Released under the [MIT license][13]



[01]: https://goreportcard.com/report/github.com/piLinux/GoREST
[11]: https://github.com/golang/go
[12]: https://github.com/gin-gonic/gin
[13]: https://github.com/piLinux/GoREST/blob/master/LICENSE
[21]: https://github.com/jinzhu/gorm
[31]: https://goapi.pilinux.me/api/v1/users
[32]: https://getpostman.com
[41]: https://github.com/piLinux/HowtoCode/blob/master/Golang/1.Intro/Installation.md
[51]: https://github.com/joho/godotenv


[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2FpiLinux%2FGoREST.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2FpiLinux%2FGoREST?ref=badge_large)