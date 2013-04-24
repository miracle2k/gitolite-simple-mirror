[Gitolite][] has very [advanced mirroring features][Mirroring], but they can
be tedious to use if you are just looking to mirror push some repositories to
Github (*).

Here is an alternative *post-receive* hook that doesn't bother with
master/slave declarations, and just lets you define a target repository
to which to mirror like so:

    repo    foo-project
            RW+     =   me
            config gitolite.mirror.simple   =   "git@github.com:miracle2k/foo-project.git"
            
It's also possible to mirror multiple repositories in one go:

    repo    @public-projects
            config gitolite.mirror.simple   =   "git@github.com:miracle2k/REPO.git"
            
As you can see, REPO will be replaced by the repository name. This is done 
within the hook.

To make the above configuration work, you need to do the following:

1. Edit *~/.gitolite.rc* to allow the config keys, for example like so:
    
        $GL_GITCONFIG_KEYS = "gitolite.mirror\..*";
    
    See the [Gitolite docs][Security] for more on the setting.

2. Setup public SSH authentication from your Gitolite server to the mirror
   target. Don't forget to connect at least once to pass SSH host key
   verification.
   
3. Copy the *post-receive* hook from this repository to
   *~/.gitolite/hooks/common*, see the [Gitolite docs][Hooks] for more.
   Make sure the file is executable.
   
4. Run *gl-setup* (as the user serving gitolite) once to have the hook
   installed in all repositories.


(*) Or even impossible if the push target does not support the
"host:repo" syntax of gitolite; gitolite's mirroring support is
primarily intended to mirror between multiple gitolite instances.


[Gitolite]: http://sitaramc.github.com/gitolite/
[Mirroring]: http://sitaramc.github.com/gitolite/mirrsetup.html
[Hooks]: http://sitaramc.github.com/gitolite/hooks.html#customhooks
[Security]: http://sitaramc.github.com/gitolite/rc.html#rcsecurity
