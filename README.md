Project status: WIP, Maildiranasaurus is still hatching! Almost done...


[![Build Status](https://travis-ci.org/flashmob/MailDiranasaurus.svg?branch=master)](https://travis-ci.org/flashmob/MailDiranasaurus)


# Maildiranasaurus

![Dino](/dino.png)

## RoaaaarrrRRRRR!

This started as a project to get an insight into using [go-guerrilla](https://github.com/flashmob/go-guerrilla) as a package.

It is uses go-guerrilla with an Maildir backend. See [serve.go](https://github.com/flashmob/maildiranasaurus/blob/master/cmd/maildiranasaurus/serve.go) how the Maildir processor was added!

The Maildir processor repo lives here: https://github.com/flashmob/maildir-processor

## Building

You'll need GNU make and Go installed

     $ make maildirasaurus

## Running

copy maildiranasaurus.conf.sample to maildiranasaurus.conf
customize it to how you like it, then:

`./maildiranasaurus serve`

## Config

Customize your config as outlined in the readme: https://github.com/flashmob/go-guerrilla 

#### Maildir

To enable maildir, customize your backend_config like so:

    "backend_config" :
    {
        "process_stack": "HeadersParser|Debugger|Hasher|Header|MailDir",
        "maildir_user_map" : "test=1002:2003,guerrilla=1001:1001,flashmob=1000:1000",
        "maildir_path" : "/home/[user]/Maildir",
        "save_workers_size" : 1,
        "primary_mail_host":"sharklasers.com",
        "log_received_mails" : false
    },
    
`process_stack` - a line of stacked processors (decorators) that work on the email envelope. 
Working from left to right, i.e. mail will be saved using the MailDir processor

`maildir_user_map` - user settings. `<username>=<user id>:<group id>` comma separated. Use -1 for `<id>` & `<group>` if you want to ignore these, otherwise get these numbers from /etc/passwd

`maildir_path` - the `[user]` part will be replaced with with the actual user from maildir_user_map once the config is loaded. Usually, no need to change this as the default is conventional. 

`save_workers_size` - how many dinosaur workers to spawn. Roaaaar!

#### Fast CGI (fcgi)

FastCGI you say? Yes, an example of the FastCGI processor is included too.
Useful if you want to deliver your emails to a php script (or other fcgi gateway)

Include the following fields in the "backend_config" object:


    "backend_config" :
    {
    
        "process_stack": "HeadersParser|Debugger|Hasher|Header|MailDir|FastCGI",
    
        // [other fields here]
    
        "fcgi_script_filename_save" : "/path/to/fastcgi-processor/examples/save.php",
        "fcgi_script_filename_validate" : "/path/to/fastcgi-processor/examples/validate.php",
        "fcgi_connection_type" : "unix",
        "fcgi_connection_address" : "/path/to/php7.0-fpm.sock"
    },
    
`fcgi_script_filename_save` - path to save script file.
    
`fcgi_script_filename_validate` - path to validate file.
    
`fcgi_connection_type` - connection type, unix or tcp
    
`fcgi_connection_address` - path to the unix socket, or <ip-address>:<port>

For scripting details, read more documentation about it here [FastCGI processor](https://github.com/flashmob/fastcgi-processor)
