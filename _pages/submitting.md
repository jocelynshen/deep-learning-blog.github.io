---
layout: page
title: submitting
permalink: /submitting
description:
nav: true
nav_order: 3
---


> **Announcement**: the submission deadline has been slightly modified. 
> - **February 2nd AOE** is now an *abstract deadline*; please submit this on [OpenReview](https://openreview.net/group?id=ICLR.cc/2023/BlogPosts&referrer=%5BHomepage%5D(%2F)).
> - **February 10th AOE** is the deadline for any modifications to your blog posts (via a [pull request on github](https://github.com/iclr-blogposts/staging/pulls)).
> - **April 28th AOE** is the deadline for the camera-ready submission. Please follow the instructions [here]({{ '/submitting#camera-ready-instructions' | relative_url }}).

### A more open process

For this edition of the Blogposts Track, we will forgo the requirement for total anonymity. 
The blog posts **must be anonymized for the review process**, but users will submit their anonymized blog posts via a pull request to a staging repository (in addition to a submission on OpenReview).
The post will be merged into the staging repository, where it will be deployed to a separate Github Pages website. 
Reviewers will be able to access the posts directly through a public url on this staging website, and will submit their reviews on OpenReview.
Reviewers should refrain from looking at the git history for the post, which may reveal information about the authors.

This still largely follows the Double-Blind reviewing principle; it is no less double-blind than when reviewers are asked to score papers that have previously been released to [arXiv](https://arxiv.org/), an overwhelmingly common practice in the ML community.
This approach was chosen to lower the burden on both the organizers and the authors; last year, many submissions had to be reworked once deployed due to a variety of reasons.
By allowing the authors to render their websites to Github Pages prior to the review process, we hope to avoid this issue entirely. 
We also avoid the issue of having to host the submissions on a separate server during the reviewing process.

However, we understand the desire for total anonymity. 
Authors that wish to have a fully double-blind process might consider creating new GitHub accounts without identifying information which will only be used for this track.
For an example of a submission in the past which used an anonymous account in this manner, you can check out the [World Models blog post (Ha and Schmidhuber, 2018)](https://worldmodels.github.io/) and the [accompanying repository](https://github.com/worldmodels/worldmodels.github.io).

### Template

The workflow you will use to participate in this track should be relatively familiar to you if have used [Github Pages](https://pages.github.com/). Specifically, our website uses the [Al-Folio](https://github.com/alshedivat/al-folio) template.
This template uses Github Pages as part of its process, but it also utilizes a separate build step using [Github Actions](https://github.com/features/actions) and intermediary [Docker Images](https://www.docker.com/).

**We stress that you must pay close attention to the steps presented in this guide. 
Small mistakes here can have very hard-to-debug consequences.**

### Contents

- [Quickstart](#quickstart)
- [Download the Blog Repository](#download-the-blog-repository)
- [Creating a Blog Post](#creating-a-blog-post)
- [Local Serving](#local-serving)
   - [Method 1: Using Docker](#method-1-using-docker)
   - [Method 2: Using Jekyll Manually](#method-2-using-jekyll-manually)
      - [Installation](#installation)
      - [Manual Serving](#manual-serving)
- [Submitting Your Blog Post](#submitting-your-blog-post)
- [Reviewing Process](#reviewing-process)
- [Camera Ready](#camera-ready-instructions)


### Quickstart

This section provides a summary of the workflow for creating and submitting a blog post. 
For more details about any of these steps, please refer to the appropriate section.


1. Fork or download our [staging repository](https://github.com/iclr-blogposts/staging). 
    We stress that you work with the [staging repository](https://github.com/iclr-blogposts/staging), not the main repository.
    - If you do fork this repo, rename your fork. You probably should rename it
    using a personalized name inspired by the subject of your submission. This is a **project** website, not a **user** website.
    - If you wish to deploy the website on your own account before submitting a pull request, follow the [deployment instructions](https://github.com/iclr-blogposts/staging/blob/master/README.md#deployment) in the README **very carefully**. Pay particular attention to the instructions detailing how you must edit the `_config.yml`.

    Note that any pull request to our repo will only permit modifying certain files, so you may have to omit some changes during the pull request.

2. Create your blog post content as detailed in the [Creating a Blog Post](#creating-a-blog-post) section.
    In summary, to create your post, you will: 
    - Create a markdown file in the `_posts/` directory with the format `_posts/2022-12-01-[SUBMISSION NAME].md`. **Please ensure to use the provided `2022-12-01-distill-example.md` (with the distill layout) as your template**.
    - Add any static image assets will be added to `assets/img/2022-12-01-[SUBMISSION NAME]/`.
    - Add any interactive HTML figures will be added  to `assets/html/2022-12-01-[SUBMISSION NAME]/`. 
    - Put your citations into a bibtex file in `assets/bibliography/2022-12-01-[SUBMISSION NAME].bib`. 

    You **should not** touch anything else in the blog post.
    Read the [relevant section](#creating-a-blog-post) for more details.
    **Make sure to omit any identifying information for the review process.**

3. To render your website locally, you can build a docker container via `$ ./bin/docker_build_image.sh` to serve your website locally. 
    You can then run it with `$ ./bin/docker_run.sh`.
    Alternatively, you can setup your loval environment to render the website via conventional `$ bundle exec jekyll serve` commands. 
    More information for both of these configuratoins can be found in the [Local Serving](#local-serving) section.

4. When ready to submit, open a pull request to our [staging repository](https://github.com/iclr-blogposts/staging). Your PR may only add files specified as specified in the [Creating a Blog Post](#creating-a-blog-post) section. Any modification to any other files will require you to undo or omit these changes.
See the section on [submitting your blog post](#submitting-your-blog-post) for more details. 

5. If accepted, we will then merge the accepted posts to our main repository. See the [camera ready](#camera-ready) section for more details on merging in an accepted blog post.

**Should you edit ANY files other than `_config.yml`, your new post inside the `_posts` directory, and your new folder inside the `assets` directory,
your pull requests will automatically be ignored.**

### Download the Blog Repository

Download or fork our [staging repository](https://github.com/iclr-blogposts/staging). 
You will be submitting a pull request to this staging repository, so if you use the fork approach, we stress that you must fork the [staging repository](https://github.com/iclr-blogposts/staging), not the main repository!

This is in contrast to last year's Blog Post track, where we explicitly stated you should *not* fork our repository.

### Creating a Blog Post

The bulk of your blogpost will be written in a Markdown file
You can check out a [sample blogpost]( {{site.url}}/{{site.posturl}}/blog/2022/09/01/distill-example), which was
generated by the markdown file in `_posts/2022-12-01-distill-example.md`.
**Please ensure that you use the distill layout in your submission.**
You must modify the file's header (or 'front-matter') as needed.

 ```markdown
---
layout: distill
title: [Your Blog Title]
description: [Your blog's abstract - a short description of what your blog is about]
date: 2022-12-01
htmlwidgets: true

# anonymize when submitting 
authors:
  - name: Anonymous 

# do not fill this in until your post is accepted and you're publishing your camera-ready post!
# authors:
#   - name: Albert Einstein
#     url: "https://en.wikipedia.org/wiki/Albert_Einstein"
#     affiliations:
#       name: IAS, Princeton
#   - name: Boris Podolsky
#     url: "https://en.wikipedia.org/wiki/Boris_Podolsky"
#     affiliations:
#       name: IAS, Princeton
#   - name: Nathan Rosen
#     url: "https://en.wikipedia.org/wiki/Nathan_Rosen"
#     affiliations:
#       name: IAS, Princeton 

# must be the exact same name as your blogpost
bibliography: 2022-12-01-distill-example.bib  

# Add a table of contents to your post.
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
toc:
  - name: [Section 1]
  - name: [Section 2]
  # you can additionally add subentries like so
    subsections:
    - name: [Subsection 2.1]
  - name: [Section 3]
---

# ... your blog post's content ...
```

You must change the `title`, `discription`, `toc`, and eventually the `authors` fields (**ensure that the
submission is anonymous for the review process**).

<!-- Add any tags that are relevant to your post, such as the areas your work is relevant to. -->
Read our [sample blog post]({{ '/blog/2022/distill-example' | relative_url }}) carefully to see how you can add image assets, and how to write using $$\LaTeX$$!
Read about rendering your post locally [below](#serving).

**Important: make sure your post is completely anonymized before you export and submit it!**

Before going any further, it will be useful to highlight exactly what folders and files you are going to add or modify.
Even if you use one of our simpler quickstart methods, this will always be what's happening 
behind the scenes.

If you clone our repo or download a release, you will find a directory structure that looks like 
the following (excluding all files and directories that are not relevant to your submission): 

```bash
your_blogpost_repo/
│
├── _posts
│   ├── 2022-12-01-[YOUR SUBMISSION].md         # <--- Create this markdown file; this is your blogpost
│   └── ...
├── assets
│   ├── bibliography
│   │   ├── 2022-12-01-[YOUR SUBMISSION].bib    # <--- Create this bibtex file
│   │   └── ...
│   ├── html
│   │   ├── 2022-12-01-[YOUR SUBMISSION]        # <--- Create this directory and add interactive html figures
│   │   │   └──[YOUR HTML FIGURES].html
│   │   └── ...
│   ├── img
│   │   ├── 2022-12-01-[YOUR SUBMISSION]        # <--- Create this directory and add static images here
│   │   │   └──[YOUR IMAGES].png
│   │   └── ...
│   └── ...
└── ...
```

In summary, to create your post, you will: 

- Create a markdown file in the `_posts/` directory with the format `_posts/2022-12-01-[SUBMISSION NAME].md`. 
- Add any static image assets will be added to `assets/img/2022-12-01-[SUBMISSION NAME]/`.
- Add any interactive HTML figures will be added  to `assets/html/2022-12-01-[SUBMISSION NAME]/`. 
- Put your citations into a bibtex file in `assets/bibliography/2022-12-01-[SUBMISSION NAME].bib`. 

You **should not** touch anything else in the blog post.

Note that `2022-12-01-[YOUR SUBMISSION]` serves as a tag to your submission, so it should be the
same for all three items.
For example, if you're writing a blog post called "Deep Learning", you'd likely want to make your
tag `2022-12-01-deep-learning`, and the directory structure would look like this:

```bash
your_blogpost_repo/
│
├── _posts
│   ├── 2022-12-01-deep-learning.md         # <--- Create this markdown file; this is your blogpost
│   └── ...
├── assets
│   ├── bibliography
│   │   ├── 2022-12-01-deep-learning.bib    # <--- Create this bibtex file
│   │   └── ...
│   ├── html
│   │   ├── 2022-12-01-deep-learning        # <--- Create this directory and add interactive html figures
│   │   │   └──[YOUR HTML FIGURES].html
│   │   └── ...
│   ├── img
│   │   ├── 2022-12-01-deep-learning        # <--- Create this directory and add static images here
│   │   │   └──[YOUR IMAGES].png
│   │   └── ...
│   └── ...
└── ...
```

### Local serving

So far we've talked about how to get the relevant repository and create a blog post conforming to our requirements.
Everything you have done so far has been in Markdown, but this is not the same format as web content (typically HTML, etc.).
You'll now need to build your static web site (which is done using Jekyll), and then *serve* it on some local webserver in order to view it properly.
We will now discuss how you can *serve* your blog site locally, so you can visualize your work before you open a pull request on the staging website so you can submit it to the ICLR venue.

#### Method 1: Using Docker 

To render your website locally, we follow the instructions for [Local setup using Docker (Recommended on Windows)](https://github.com/iclr-blogposts/iclr-blogposts.github.io/blob/master/README.md#local-setup-using-docker-recommended-on-windows), but specifically you will need to create your own docker container rather than pull it from Dockerhub (because we modified the Gemfile).

In summary, the steps are as follows:

1. Create your Docker image:

    ```
    ./bin/docker_build_image.sh
    ```

    Remove the `Gemfile.lock` file if prompted.
    This will create a docker image labeled as `al-folio:latest`. 

2. Run the Docker image:

    ```
    ./bin/docker_run.sh
    ```

    Remove the `Gemfile.lock` file if prompted. 
    Don't use `dockerhub_run.sh`; this may result in issues with missing jekyll dependencies.


#### Method 2: Using Jekyll Manually

For users wishing to not use a Docker container, you can install Jekyll directly to your computer and build the site using Jekyll directly.
This is done at your own risk, as there are many potential points of error!
Follow the instructions for rendering the website via the conventional method of `$ bundle exec jekyll serve`

##### Installation

You will need to manually install Jekyll which will vary based on your operating system.
The instructions here are only for convenience - you are responsible for making sure it works on your system and we are not liable for potential issues that occur when adding your submissions to our repo!

**Ubuntu/Debian**

1. Install Ruby

    ```bash
    sudo apt install ruby-full
    ```

2. Once installed, add the following to your `.bashrc` or whatever terminal startup script you may use (this is important because otherwise gem may complain about needing sudo permission to install packages):

    ```bash
    export GEM_HOME="$HOME/.gem"
    export PATH="$HOME/.gem/bin:$PATH"
    ```

3. Install Jekyll and Bundler:

    ```bash
    gem install jekyll bundler
    ```

**MacOS and Windows**

Mac and Windows users can find relevant guides for installing Jekyll here:

- [Windows guide](https://jekyllrb.com/docs/installation/windows/)
- [MacOS guide](https://jekyllrb.com/docs/installation/macos/)

##### Manual Serving

Once you've installed jekyll and all of the dependencies, you can now serve the webpage on your local machine for development purposes using the `bundle exec jekyll serve` command.

You may first need to install any project dependencies. In your terminal, from the directory containing the Jekyll project run:

```bash
bundle install
```

This will install any plugins required by the project. 
To serve the webpage locally, from your terminal, in the directory containing the Jekyll project run:

```bash
bundle exec jekyll serve
```

You should see something along the lines of:

```
> bundle exec jekyll serve
Configuration file: /home/$USER/blog_post_repo/_config.yml
            Source: /home/$USER/blog_post_repo
       Destination: /home/$USER/blog_post_repo/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
       Jekyll Feed: Generating feed for posts

        ... you may see a lot of stuff in here related to images ...

                    done in 0.426 seconds.
 Auto-regeneration: enabled for '/home/$USER/blog_post_repo'
    Server address: http://127.0.0.1:4000/2023/
  Server running... press ctrl-c to stop.
```

If you see this, you've successfully served your web page locally!
You can access it at server address specified, in this case `http://127.0.0.1:4000/2023` (and the blog posts should once again be viewable at the `blog/` endpoint).


### Submitting your Blog Post

The submission steps are as follows:

1. Strip all identifying information from your blog post, such as your names, instituitions, etc. 
    Be mindful that your commit history may include identifying history (beyond your Github usernames); 
    this is okay as reviewers are only permitted to look at the live blog post and not the source repository during the review process, 
    however if this is important to you, you may consider to rebase your commits (not required).

2. Make a new Pull Request to the [staging repository](https://github.com/iclr-blogposts/staging/pulls) (not the 2023 repo!) containing your blog post.
    Recall that your changes should (at most) modify the following files and directories:
    ```bash
    your_blogpost_repo/
    │
    ├── _posts
    │   ├── 2022-12-01-deep-learning.md         # <--- Create this markdown file; this is your blogpost
    │   └── ...
    ├── assets
    │   ├── bibliography
    │   │   ├── 2022-12-01-deep-learning.bib    # <--- Create this bibtex file
    │   │   └── ...
    │   ├── html
    │   │   ├── 2022-12-01-deep-learning        # <--- Create this directory and add interactive html figures
    │   │   │   └──[YOUR HTML FIGURES].html
    │   │   └── ...
    │   ├── img
    │   │   ├── 2022-12-01-deep-learning        # <--- Create this directory and add static images here
    │   │   │   └──[YOUR IMAGES].png
    │   │   └── ...
    │   └── ...
    └── ...
    ```
    Your PR will be briefly reviewed to ensure that it matches the formatting requirements (no content review), and it will then be merged into the staging version of the blog.

3. Submit the name of your blog post and its URL to our [OpenReview](https://openreview.net/group?id=ICLR.cc/2023/BlogPosts&referrer=%5BHomepage%5D(%2F)).

> **Note:** the abstract deadline preceeds the PR deadline and you might not have a PR merged before this.
> As a result, you may not have a URL ready for the abstract deadline; please do your best to estimate your URL. 
> It will be created with the following format: 
> ```
> https://iclr-blogposts.github.io/staging/blog/2022/<YOUR-BLOGPOST-NAME>/
> ```
> Using the example above, if your blog post's file is `2022-12-01-deep-learning.md`, then the corresponding url will be:
> ```
> https://iclr-blogposts.github.io/staging/blog/2022/deep-learning/
> ```
> Note that if you render your post locally, you will be able to see how the URL of your post is formatted (but please use the correct base url of `https://iclr-blogposts.github.io/staging`).
> We will be fairly accomodating about this if any issues arise once your submission is merged.

### Reviewing Process

Reviewers will be required to only view the live content of the blog. 
We ask that they act in good faith, and refrain from digging into the repository's logs and closed Pull Requests to find any identifying information on the authors.

### Camera-ready instructions 

To streamline the process of merging the accepted posts into the final blog post site, we have prepared a branch with all of the accepted blog posts in the staging repo which can be found here: 
- [https://github.com/iclr-blogposts/staging/tree/accepted](https://github.com/iclr-blogposts/staging/tree/accepted)

Please fetch this branch, and proceed with adding any final changes by creating a branch from accepted, and then merge your changes by opening a PR against this branch. 
The checklist for updating your blog post is as follows:

1. Implement any required changes from the review stage
    - If you had a conditional acceptance, ensure that you update your post following the feedback given.
2. Deanonymize your post
    - Update the author list + any links that were anonymized for the review process
3. Update formatting 
    - **Abstracts:** ensure that your abstracts are contained within the `description` entry of the front-matter, so it renders correctly in the blog ([example](https://github.com/iclr-blogposts/staging/blob/aa15aa3797b572e7b7bb7c8881fd350d5f76fcbd/_posts/2022-12-01-distill-example.md?plain=1#L4-L))
    - **Table of contents:** you must use the `toc` formatting like that in the distill template ([example](https://github.com/iclr-blogposts/staging/blob/aa15aa3797b572e7b7bb7c8881fd350d5f76fcbd/_posts/2022-12-01-distill-example.md?plain=1#L33-L42))
    - **Bibliography:** uses correct reference style as per the distill template (i.e. using the bibtex file)

Once you have updated your blog post with any necessary changes:

- Open a pull request against the accepted branch of the staging repo. 
- You should see a PR template when you open up a PR - please fill it in and make sure all of the required boxes are ticked before submitting your final PR. 


Below is what you should see in the PR template:

```md
<!-- Please make sure you are opening a pull request against the `accepted` branch (not master!) of the STAGING repo (not 2023!) -->

## OpenReview Submission Thread
<!-- link to your OpenReview submission -->

## Checklist before requesting a review
<!-- To tick a box, put an 'x' inside it (e.g. [x]) -->

- [ ] I am opening a pull request against the `accepted` branch of the `staging` repo
- [ ] I have de-anonymized my post, added author lists, etc.
- [ ] My post matches the formatting requirements
	- [ ] I have a short 2-3 sentence abstract in the `description` field of my front-matter 
	- [ ] I have a table of contents, formatted using the `toc` field of my front-matter 
	- [ ] My bibliography is correctly formatted, using a `.bibtex` file as per the sample post

## Changes implemented in response to reviewer feedback

- [ ] Tick this box if you received a conditional accept
- [ ] I have implemented the necessary changes in response to reviewer feedback (if any)

<!-- briefly add your changes in response to reviewer feedback -->

## Any other comments


```