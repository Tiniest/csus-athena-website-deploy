# Deploy Static Sites To Athena Using rsync

This repo is a template for you to setup easy deployments to the CSUS Athena server using `rake`.

## Your ECS Account

Your ECS account can be accessed here: https://www.ecs.csus.edu/index.php?content=ecs_portal

On that page is a `Get an ECS Account` button where you can request for an ECS account.


## Preparing The Athena Server

Your static site is accessed through `http://athena.ecs.csus.edu/~YOUR_ECS_NAME/`.

For example, mine is http://athena.ecs.csus.edu/~adenwalm/

The site serves you `/html` folder. The folder permissions for this folder are set as `drwx--s--x`. By default, Athena has a default html folder, we will need to remove that.

SSH into `Athena`: `$ ssh YOUR_ECS_NAME@athena.ecs.csus.edu`

Rename the html folder: `$ mv html html_backup`

Create a new html folder: `$ mkdir html`

Update html folder permissions: `$ chmod 711 html/`

Exit Athena: `$ exit`


## Deploying Your Server:

Update the `index.html` to your info

In the project root folder (not the `src` folder), deploy using `rake`: `$ rake deploy ATHENA_USER=YOUR_ECS_NAME` (Be sure this is on your local machine!)

View your site on `http://athena.ecs.csus.edu/~YOUR_ECS_NAME/`!


## How It Works

The rake command looks for the `src` directory and uses rsync to sync the contents onto the Athena server's `/html` directory. It  requires the `ATHENA_USER` env in order to rsync your data. 

The full command is: `rsync -avze 'ssh' --delete src/ ATHENA_USER@athena.ecs.csus.edu:/gaia/class/student/ATHENA_USER/html/`

`rsync`: Rsync doesn't transfer everything, it compares the source and the destination and updates what is different

`-a`: Recursively copy all files

`v`: Runs in verbose mode allowing us to see which files are transferred

`z`: Compress files to minimize transfer size

`e 'ssh'`: Sets the remote shell command to be `ssh` (network protocol)

`--delete`: deletes files that don't exist on the source

`src/`: source directory

`ATHENA_USER@athena.ecs.csus.edu`: The user and the host that you want to connect to (`ATHENA_USER` is your username)

`/gaia/class/student/ATHENA_USER/html/`: This is the destination we want to sync our files to. Run `$ pwd` on the Athena server to get this path


## Permissions Issue

In some cases, you may need to update the permissions of your deployed files on the first deploy. (It should be fine after that). To do so, ssh into Athena, cd into the html dir `$ cd html/` and run `$ chmod 744`.

Please let me know if you run into any other issues so we can take a look and documment it.


## Credits

Thank you [@mgomez0]( https://github.com/mgomez0 ) for testing out this guide.
