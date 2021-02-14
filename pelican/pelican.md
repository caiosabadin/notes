# Pelican Basics #

Here is a straightforward guide regarding Pelican, a static blog generator written in Python.

Refer to [the official Pelican docs](https://docs.getpelican.com/) for more information.

## Installing ##

Run:

```bash
python -m pip install "pelican[markdown]"
```

## Creating ##

Create a directory for your blog. In your shell, go to the directory. Then run:

```bash
pelican-quickstart
```

## Theme ##

From [a list of Pelican themes](http://pelicanthemes.com/), download your preferred one.

Add it to your Pelican project with the following command, which must be executed in your Pelican's project root directory:

```bash
pelican-themes -i /path/to/the/theme/directory/
```

Also in the project's root directory, use the command below to check if the theme was successfully registered within your Pelican project, and to display its name:

```bash
pelican-themes -l
```

Then edit the file `pelicanconf.py` in your project's root directory, adding the following line to it, in order to specify the theme your blog is going to use:

```bash
THEME = 'the_theme_name'
```

After that, run this command for the configurations to take effect:

```bash
pelican content
```

## Bloggin' ##

Pelican works with Markdown files.

Write your post in that format and save the file as a `.md` one, under the `content` directory of your project's root directory.

Below is a post example:

```md
Title: Hello, world!
Date: 2021-02-13 17:07
Category: News

Hello, world! This is my first Pelican post.
```

After writing it, run this in order to publish the content:

```bash
pelican content
```

Should you inform no value for `Category`, it will be specified either after the name of the directory the post is in, if `USE_FOLDER_AS_CATEGORY` is set to true, or according to the value defined on `DEFAULT_CATEGORY`, whose default value is `misc`.

Those settings might be changed by editing the `pelicanconf.py` in your project's root directory.

Provided those settings are missing, you can add them yourself to the `pelicanconf.py` file:

```md
USE_FOLDER_AS_CATEGORY = False
DEFAULT_CATEGORY = 'Uncategorised'
```

## Running ##

To preview your blog, in your project's root directory, run:

```pelican --listen```

If you want to actualise the content whilst the server is running, add the `-r` parametter:

```pelican -r --listen```

## Hosting ##

Whenever you run a `pelican content` or do any changes when you are running `pelican -r --listen`, your `output` directory will be updated.

To host your blog, you just have to copy the content of that `output` directory and put it in your server.
