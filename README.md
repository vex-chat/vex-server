# vex-server

A websocket powered chat server, written in go. Uses ed25519 signing for user authentication. Aims to eventually provide a secure, an easy to use, end to end encrypted messaging backend for small to medium groups.

Currently supports:

- user registration
- nick changes
- public chat channels
- private chat channels
- basic moderation (kicking, banning)
- basic permission configuration

You can set the power levels required for various moderation actions in the `config.json` file.

## Installing

Simply download the executable and run it.

```
wget https://github.com/ExtraHash/vex-server/releases/download/v0.1.0/vex-server
./vex-server
```

### Supported databases

By default, the backend will create an sqlite3 database. 

You can also use a mysql database by modifying the `dbType` and `dbConnectionStr` fields in the generated config.json file. See the example configs in the example folder.

The backend may also work with other gorm supported databases, but have not been tested.

## Compiling From Source

```
git clone git@github.com:ExtraHash/vex-server
cd vex-server
go build
./vex-server
```

## Installation

1. Download the latest binary from the [releases page](https://github.com/ExtraHash/vex-server/releases)
2. Run the binary with `./vex-server`
3. Log in with a client into your server
4. Shut down the server, open the vex-server.db database file, and set your user to 100 power level (this will eventually be automated in the install process)
5. Start the server back up.
6. The server is now up and running on port 8000.

### Different Databases

The server will automatically start up with sqlite3, but you can configure it to use mysql. See the example configurations in the `example` folder.

Note to run mysql, you will first need to create the database and apply this command to it:

```sql
ALTER DATABASE
    database_name
    CHARACTER SET = utf8mb4
    COLLATE = utf8mb4_unicode_ci;
```

### Confiuration

On the first run, it will generate a config.json file in the working directory if one is not present. The configuration is always loaded from this json file. You can change the server message, public registration, and the moderation power levels. This is the default configuration file.

```json
{
    "welcomeMessage": "Welcome to the chat server.",
    "dbType": "sqlite3",
    "dbConnectionStr": "vex-server.db",
    "publicRegistration": true,
    "port": 8000,
    "powerLevels": {
        "kick": 25,
        "ban": 50,
        "op": 100,
        "grant": 50,
        "revoke": 50,
        "talk": 0,
        "create": 50,
        "delete": 50,
        "files": 25
    }
}
```

## Command Line Arguments

Various utilities are available by launching the binary with specific command line arguments. You may also define a custom keyfolder, config path, or port here.

```
--keys /path/to/keyfolder         The path that your key folder is in. If it is not present, one will be generated.
--config /path/to/config.json     The path that your config file is in. If it is not present, the default config will be written here.
--register PubKey PowerLevel      Registers a user manually on the server. with a given public key and power level. Will print the user details.
--promote PubKey PowerLevel       Modifies a registered user's power level.
--port number                     Sets the port number to listen on.
--backup /path/to/file.json       Dumps the database into JSON for import into another Vex Server.
--import /path/to/file.json       Imports the JSON dump generated by another Vex Server.
```
