How to modify the wiki ? 
-----------------------------
* Click on 'clone or download' and copy the link 

* On your terminal, type: 
`$ git clone https://github.com/hut34/wiki.git`

* Create a branch     
`$ git checkout -b BRANCH_NAME`

* To modify the content of the wiki: 
1. `cd wiki`
2. Initialize and start Slate. To do this: 
```shell
bundle install
bundle exec middleman server
```
You can now see the docs at http://localhost:4567. Whoa! That was fast!

Now that Slate is all set up on your machine, you'll probably want to learn more about [editing Slate markdown](https://github.com/lord/slate/wiki/Markdown-Syntax), or [how to publish your docs](https://github.com/lord/slate/wiki/Deploying-Slate).


Once done, you have commit and push.
```shell
git add /directory/FILENAME
```
FOR ADMINISTRATOR ONLY
----------------------
To review the work done by others before merging: 
```shell
git fetch
git checkout BRANCH_NAME
```
Then run it locally and check for errors



Get started with Slate
------------------------------

### Prerequisites

You're going to need:

 - **Linux or macOS** — Windows may work, but is unsupported.
 - **Ruby, version 2.3.1 or newer**
 - **Bundler** — If Ruby is already installed, but the `bundle` command doesn't work, just run `gem install bundler` in a terminal.


