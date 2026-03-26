# Repo2026
Author - Samiran Das

--> Git Configuration <br>
1. git config --global user.name "Repo2026" (sets user name)
2. git config --list
3. git --version
4. clear
5. Clone git repository in our local machine --> git clone (paste the https link got from Repo -> <> Code section) <_|
6. Change Directory or go into a folder - > CD folderName
7. View all files --> ls
8. view hidden files --> ls -a
9. Status of your brunch -> git status
10. To restore name --> git restore <fileName>
11. fetch all data --> git fetch [Downloads changes from the remote (GitHub) but does NOT merge them into your local branch. Your working files stay untouched. You're just "checking what's new."]
12. Pull all data from Main/Origin / Git --> git pull [Downloads changes AND automatically merges them into your current branch. It's essentially git fetch + git merge in one command.]
13. Add changes or stage --> git add <file name or . for all>
14. Commit --> git commit -m "<Comment>"
15. Push changes from Local to Remote --> git push origin main
16. Move out from Directory --> cd ..
17. New Repo / Folder --> mkdir <Folder Name>
18. Make the local repo as Git Repo --> git init
19. Add Remote repo o the github / repository --> create a new Repo on Git --> git remote add origin < -link- >
20. Verify Remote / check if the git setup was ok or not --> git remote -v
21. Check brunch --> git branch
22. default branch --> Master
23. change branch name --> git branch -M <Name>
24. future sort form for push --> git push -u origin main (It will always push in main from future)
25. git stash --> Temporarily saves uncommitted changes so you can switch branches cleanly.
26. git stash        # saves changes
27. git stash pop    # restores them back
28. git stash list   # see all stashed changes

Branch Commands
1. Check brunch --> git branch
2. change branch name --> git branch -M <Name>
3. Create branch --> git checkout -b <New Branch Name>
4. Switch Branch --> git checkout <Branch Name>
5. Delete Branch --> git branch -d <Branch Name>
6. Compare branch --> git diff <Other Branch>
7. marge changes with other branch --> git marge <Other Branch Name>
8. At the time of remote pull a change present already in your branch then try using marge or replace 
--> git config pull.rebase false (for Marge / make it true for replace)
9. Directly Merge 2 branches from vs code --> git merge <Destinaion branch>
10. If we have changes on file and made git add after that if we want to reset --> git Reset <file name>

Undo <br>
11. If we have done 1 commit and want to do reset the changes --> git reset HEAD~1 <br>
12. Check commit log --> git log --> to quite --> Press Q <br>
13. For doing Multiple commit undoing --> git reset <HASH / take the id from Log> <br>
14. --> git reset --hard <HASH / take the id from Log> --> it will remove the changes not only from Git also from the Vs code.<br>
15. git cherry-pick <commit-hash> lets you pick that specific commit and apply it wherever you are (Imagine you have a bug fix commit on your feature branch, and you want just that one commit on main — without merging the whole branch)<br>
16. git reset --soft HEAD~1   # undoes commit, keeps changes staged<br>
18. git reset --mixed HEAD~1  # undoes commit, keeps changes unstaged (default)<br>
19. git reset --hard HEAD~1   # undoes commit, DELETES changes permanently ⚠️<br>
20. git revert <commit-hash> — Instead of erasing history, it creates a brand new commit that undoes the changes of a previous commit. History is preserved!<br>

How to make project copy or Fork ---<br>
Open the repo --> Click on Fork --> Create Fork<br>












https://www.instagram.com/reel/DUQhv8CiLSB/?utm_source=ig_web_copy_link&igsh=NTc4MTIwNjQ2YQ==

Song - Tere Liye Mandir Jau Tere Naa - https://www.youtube.com/watch?v=johuvftT9AU&list=RDjohuvftT9AU&index=1
