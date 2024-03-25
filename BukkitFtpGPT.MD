
# Minecraft Bukkit Plugin FTP Interaction

## User's Request

The user is developing a Bukkit plugin for Minecraft version 1.12.2. They need to implement functionality for:

1. Connecting to an FTP server at a specified directory ("logs") when the server starts.
2. Fetching a player's log file (`player.txt`) from the local directory ("/private_logs/player.txt") and uploading it to the FTP server when the `/logs player` command is used. This upload should occur in a separate thread to avoid blocking the main Bukkit command processing thread. After the upload, the player should be notified that their file is successfully uploaded and available at a specified URL (constructed as `my_url + "/logs/player.txt"`).
3. Clearing the "logs" directory on the FTP server of all files when the server is shutting down.
4. Storing configuration details necessary for connecting to the FTP server, including host, user, password, port, and directory names for both the FTP server and local storage.

## Solution Outline

The task involves writing Java code for the Bukkit plugin that includes working with an FTP server. The solution is divided into several steps:

1. **FTP Connection on Server Start:** Using a library like Apache Commons Net for FTP operations and connecting to the FTP server in the plugin's `onEnable` method.
2. **`/logs player` Command Implementation:** Creating the command using the Bukkit API, fetching the local file, and uploading it to the FTP server in a separate thread.
3. **Notification After Successful Upload:** Using the Bukkit API to send a message to the player who issued the command.
4. **Clearing FTP Directory on Server Shutdown:** Connecting to the FTP and clearing the specified directory in the plugin's `onDisable` method.
5. **Plugin Configuration:** Using the plugin's `config.yml` file to store the FTP server connection details and directory names.

## Implementation Details

The implementation involves adding Maven dependencies for Apache Commons Net, implementing the plugin class that handles the FTP connection, file upload, and directory clearing, and configuring the plugin through `config.yml`.

### Key Implementation Snippets

- **Adding Dependencies:**
    ```xml
    <dependency>
        <groupId>commons-net</groupId>
        <artifactId>commons-net</artifactId>
        <version>3.6</version>
    </dependency>
    ```

- **Plugin Class Implementation:** (Java code for establishing FTP connection, handling the `/logs player` command, and clearing the FTP directory on shutdown)

- **Configuration in `config.yml`:**
    ```yaml
    ftp:
      host: "your_host"
      user: "your_user"
      pass: "your_pass"
      port: 21
      ftp_logs_directory: "logs"
      local_logs_directory: "private_logs"
    ```

---

The user then requested to export this dialogue into a `.md` file.
