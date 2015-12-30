###########################
Rakefile
###########################

*******************
create a new post
*******************

::
    
    $ rake new_post['post-title']
    $ rake new_post_tech['post-title']
    $ rake new_post_art['post-title']
    $ rake new_post_life['post-title']


custom new post commands

Rakefile - add post dir

::

    posts_dir       = "_posts"      # directory for blog files
    posts_tech_dir  = "_posts/tech" # directory for tech posts
    posts_art_dir   = "_posts/art"  # directory for art posts
    posts_life_dir  = "_posts/life" # directory for life posts
   
Rakefile - add commands

::

    # usage rake new_post_tech[my-new-post] or rake new_post_tech['my new post'] or rake new_post_tec    h (defaults to "new-post")
    desc "Begin a new post in #{source_dir}/#{posts_tech_dir}"
    task :new_post_tech, :title do |t, args|
      if args.title
        title = args.title
      else
        title = get_stdin("Enter a title for your post: ")
      end
      raise "### You haven't set anything up yet. First run `rake install` to set up an Octopress the    me." unless File.directory?(source_dir)
      mkdir_p "#{source_dir}/#{posts_tech_dir}"
      filename = "#{source_dir}/#{posts_tech_dir}/#{Time.now.strftime('%Y-%m-%d')}-#{title.to_url}.#{    new_post_ext}"
      if File.exist?(filename)
        abort("rake aborted!") if ask("#{filename} already exists. Do you want to overwrite?", ['y',     'n']) == 'n'
      end
      puts "Creating new post: #{filename}"
      open(filename, 'w') do |post|
        post.puts "---"
        post.puts "layout: post_tech"
        post.puts "title: \"#{title.gsub(/&/,'&amp;')}\""
        post.puts "date: #{Time.now.strftime('%Y-%m-%d %H:%M:%S %z')}"
        post.puts "comments: true"
        post.puts "categories: [tech]"
        post.puts "tags: "
        post.puts "---"
      end
    end


******************
Deploy
******************

::

    $ rake setup_github_pages
    $ rake generate
    $ rake deploy

    $ echo "domain_name" >> source/CNAME

if confict, just remove the folder ``_deploy``, and ``git pull origin gh-pages`` again.

::

    $ rm -rf _deploy
    $ mkdir _deploy
    $ git init
    $ git remote add origin https://xxxx
    $ git remote -v
    $ git pull origin gh-pages
    $ cd ..
    $ rake deploy
