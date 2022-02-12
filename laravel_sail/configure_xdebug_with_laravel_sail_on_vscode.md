# How to configure XDebug for Laravel Sail on Visual Studio Code

> Tested on Laravel [7,8,9]

## 1. Publish Sail's DockerFiles
```bash
my-projet$ sail up -d
my-projet$ sail artisan sail publish
```

## 2. Patch Sail's DockerFiles

> _Note : If you don't know you PHP_VERSION take a look at /docker-compose.yml file_

Edit [YOUR_PROJECT_ROOT]/docker/[YOUR_PHP_VERSION]/php.ini

Add this block at the bottom of the file:
```ini
[XDebug]
xdebug.mode = debug
xdebug.start_with_request = yes
xdebug.discover_client_host = true
xdebug.idekey = VSC
xdebug.client_host = host.docker.internal
xdebug.client_port = 9003
xdebug.log_level = 0
```

## 3. Rebuid you docker container
```bash
my-projet$ sail down
my-projet$ sail build
```

## 4. Install PHP Debug extension
Launch VS Code Quick Open (Ctrl+P), paste the following command, and press enter.
```
ext install xdebug.php-debug
```
## 5. Create VSCode debug configuration
Create a file named `[YOUR_PROJECT_ROOT]/.vscode/launch.json` and paste the content below
```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Listen for Xdebug",
      "type": "php",
      "request": "launch",
      "port": 9003,
      "pathMappings": {
        "/var/www/html" : "${workspaceFolder}"
      },
      "xdebugSettings": {
        "max_data" : 65535,
        "show_hidden": 1,
        "max_children": 100,
        "max_depth": 5
      }
    },
  ]
}
```

## 6. Enable XDebug in Sail

Edit [YOUR_PROJECT_ROOT]/.env and add
```env
SAIL_XDEBUG_MODE=debug
```

## 7. Restart sail
```bash
my-projet$ sail up
```

## 8. Debug
In Visual Studio Code,
- Set a breakpoint in a php file by clicking just before line number (a red dot appear)
- Go in debug mode (Bug & Start Icon), click start
- Browse to a page where the breakpoint will be trigged


Happy debuging !


