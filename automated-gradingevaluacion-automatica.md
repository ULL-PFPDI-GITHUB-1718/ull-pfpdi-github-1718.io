### [Jacob Fiksel (Biostatiscs Johns Hopkins College) Methodology for Grading Assignments](https://github.com/jfiksel/github-classroom-for-teachers)
Before I go into the steps on how to do this, I'll mention that this step will probably vary from teacher-to-teacher. If you have a different workflow from us, please feel free to share how you grade assignments from GitHub!

I also want to give the general outline of our grading workflow before I go into the details. The idea is that for each assignment, we will clone all of the students' assignments into our local computer using a shell script. These assignments (which are directories/repositories) will live inside a directory unique to that assignment. So, assignment1 will have an assignment1 directory, and inside of that will be many directories, one for each student's repository. We will then add comments or edit each student's homework assignment locally and save these changes. We then have a shell script that will commit these changes and push each student's repository back to GitHub with a single commit message ("Graded $date $time"). The students can then click on this commit message in their GitHub repository to see a diff and will know what you added.

Here are the steps.

1. Have a directory for the class in your local computer. Inside of this directory, I would recommend making a directory titled something similar to `homework-grading`. The way we will use this directory is that we have a shell script (more on this in the next step) that will create a sub-directory within the `homework-grading` named after an assignment prefix. The script then automatically clones all of the students' repos for this assignment into the directory.


2. Navigate to the `homework-grading` directory. The shell scripts we will be using are from https://github.com/konzy/mass_clone. You can either clone this repo, or my forked version https://github.com/jfiksel/mass_clone, into the `homework-grading` directory. Currently, my forked version allows you to pull changes in from a student's repository if they have made changes after your initial cloning. At this point, your directory structure should be as follows:

```
class-fall-2017
│
│
│
└───homework-grading
    │
    │
    │
    └───mass_clone
        │   README.me
        │   clone_all.sh
        |   clone_all_helper_example.sh
        |   push_all.sh
```

3. Edit the `clone_all_helper_example.sh` script so that your class specific organization and your username replaces the default settings in the organization and username fields

4. When you are ready to edit all the assignments, go to the terminal and navigate to the `mass_clone` repository. Type in `./clone_all_helper_example.sh`. You can then enter in the assignment prefix when prompted (or just type `./clone_all_helper_example.sh assignment-prefix`). For example, if we are grading `unit-1-homework-1`, then I would type in
`./clone_all_helper_example.sh unit-1-homework-1`. You will then be prompted to enter your GitHub password. Unfortunately, you will have to enter in the password everytime you do this step, even if you have SSH set up (which you should!). Your directory structure will now look like this

```
class-fall-2017
│
│
│
└───homework-grading
    │
    │
    │
    |────mass_clone
    |    │   README.me
    |    │   clone_all.sh
    |    |   clone_all_helper_example.sh
    |    |   push_all.sh
    |
    |
    |
    |────unit-1-homework-1
         |
         |
         |----unit-1-homework-1-student1
         |
         |
         |----unit-1-homework-1-student2
         |
                  .
                  .
                  .
         |
         |----unit-1-homework-1-studentN
```

5. You can either edit all the assignments by typing comments into their documents, or by adding a new file with comments to each student's repository. If there is only one document that students will be editing, then it's possible to open up each student's document, put in your changes, and then save. This is also nice because you can use regular expressions to open up every student's assignment without you clicking. For example, if the document is called homework1.Rmd, and you are inside of the unit-1-homework-1 directory, you can type `open */homework1.Rmd` (you can replace open with whatever command you want so that the files open in your preferred editor). You can then work through each student's assignment, saving and then closing their assignment after you are done grading. You may or may not want to put official assignment grade into your comments due to privacy issues.

6. After saving (you have to save!) your changes and/or new files, navigate to the `mass_clone` directory. Type in `./push_all.sh assignment-prefix`. This script will commit all of your changes with the same commit message ("Graded $date $time"), and then push all of the changes back to the students' repositories.


#### Herramientas que pueden ayudar en el proceso de Evaluación

#### ghedsh

1. There are different ways to clone the students repositories. I prefer to use [ghedsh](https://github.com/ULL-ESIT-GRADOII-TFG/ghedsh) for the cloning process
  `gem install ghedsh` (You need to have ruby installed). Here is an example of use:
	````bash
	[~/src/curso-gihub(master)]$ ghedsh

	GitHub Education Shell!
	_______________________

	crguezl> orgs

	etsii2
	Lenguajes-Paradigmas-Programacion-ULL
	ULL-ETSII-GRADO-SYTW-1314
	ULL-ESIT-GRADOII-PL
	etsiiull
	dwyl
	SYTW
	ULL-ESIT-GRADOII-DSI
	classroom-testing
	ULL-ESIT-GRADOII-TFG
	ULL-ESIT-SYTW-1617
	ULL-ESIT-DSI-1617
	ULL-ESIT-PL-1617
	ULL-ESIT-LPP-1617
	ULL-PFPDI-GITBOOK-1617
	ULL-ESIT-TFM-test-evaluation-shell
	ULL-ESIT-PL
	ULL-ESIT-MII-CA-1718
	ULL-ESIT-PL-1718
	ULL-PFPDI-GITHUB-1718
	ULL-LSI

	crguezl> cd ULL-LSI
	crguezl>ULL-LSI> repos

	miembros-del-area-jlroda
	miembros-del-area-fsande
	lista-de-comisiones
	miembros-del-area-esegredo
	ull-lsi.github.io
	algoritmos-y-lenguajes-paralelos

	Repositories found: 6
	crguezl>ULL-LSI> !pwd
	/Users/casiano/src/curso-gihub
	crguezl>ULL-LSI> help clone
		clone			Clone a repository.
					->	clone [repository]

					You can use a RegExp to clone several repositories with / parameter 
					->	clone /[RegExp]/

	crguezl>ULL-LSI> clone /miembros-del/
	Cloning into 'miembros-del-area-jlroda'...
	warning: You appear to have cloned an empty repository.
	Cloning into 'miembros-del-area-fsande'...
	warning: You appear to have cloned an empty repository.
	Cloning into 'miembros-del-area-esegredo'...
	warning: You appear to have cloned an empty repository.
	crguezl>ULL-LSI> exit
	[~/src/curso-gihub(master)]$ ls -ltrd miembros-del-area-*
	drwxrwxr-x  3 casiano  staff  96 21 nov 10:19 miembros-del-area-jlroda
	drwxrwxr-x  3 casiano  staff  96 21 nov 10:19 miembros-del-area-fsande
	drwxrwxr-x  3 casiano  staff  96 21 nov 10:20 miembros-del-area-esegredo
```

##### Gitomator

* [Organizationpage](https://github.com/gitomator)
* [GitHub page](https://gitomator.github.io/)
* [gitomator docs: welcome](https://gitomator.github.io/docs/welcome/)
* [Gitomator Classroom](https://github.com/gitomator/gitomator-classroom)

##### classwerk

The classwerk project is a set of scripts to help facilitate using Github as an assignment management platform.

* [keshavdv/classwerk]https://github.com/keshavdv/classwerk)

#### Classroom desktop

* [education/classroom-desktop](https://github.com/education/classroom-desktop)
 Uses electron

#### Massive cloning with `git submodule`

This is yet another way to cope with the massive cloning-pulling-pushing process.

The idea is:

1. Create a repo `eval-assignment-name`
2. Add as submodules all the student repos using the [git submodule](https://git-scm.com/docs/git-submodule) command
  ```bash
  [~/src/githubclassroom/submodules]$ ls -l eval-cuentame-un-cuento/
  total 0
  drwxr-xr-x  7 casiano  staff  224 22 nov 12:27 cuentame-un-cuento-MarysePrivat
  drwxr-xr-x  7 casiano  staff  224 22 nov 12:23 cuentame-un-cuento-cpgonzal
  drwxr-xr-x  7 casiano  staff  224 22 nov 12:27 cuentame-un-cuento-fernandostrut
  drwxr-xr-x  7 casiano  staff  224 23 nov 08:25 cuentame-un-cuento-josestevez
  ```
3. Open with your favourite editor all the students works and review them modify what is necessary.
  Mine is `vi`. The following command opens in the editor a tab per student:
  ```bash
  [~/local/src/githubclassroom/submodules/eval-cuentame-un-cuento(master)]$ vi -p cuentame-un-cuento-*/
  ```
4. Use the command [git submodule foreach shell-script](https://git-scm.com/docs/git-submodule) para hacer un push masivo a los repos de los cambios realizados:
   The command `submodule for each shell-script` evaluates an
   arbitrary shell command in each checked out submodule. 

   The command has access to the variables `$name`, `$path`, `$sha1` and `$toplevel`:
   `$name` is the name of the relevant submodule section in `.gitmodules`,
   $path is the name of the submodule directory relative to the
   superproject, $sha1 is the commit as recorded in the superproject,
   and $toplevel is the absolute path to the top-level of the
   superproject. Any submodules defined in the superproject but not
   checked out are ignored by this command. Unless given --quiet,
   foreach prints the name of each submodule before evaluating the
   command. If --recursive is given, submodules are traversed
   recursively (i.e. the given shell command is evaluated in nested
   submodules as well). A non-zero return from the command in any
   submodule causes the processing to terminate. This can be
   overridden by adding || : to the end of the command.

As an example, the command below will show the path and currently checked out commit for each submodule:

git submodule foreach 'echo $path `git rev-parse HEAD`'   

### Referencias

* [How do you use Issues in your class?](https://education.github.community/t/how-do-you-use-issues-in-your-class/8361/7)
* [Student feedback templates](https://education.github.community/t/student-feedback-templates/18767)
* [Grading posts at GitHub Education](https://education.github.community/search?q=grading)
* [How to automatically gather or collect assignments?](https://education.github.community/t/how-to-automatically-gather-or-collect-assignments/2595)
* [What tools do you use for automated grading of assignments that involve programming?](https://www.researchgate.net/post/What_tools_do_you_use_for_automated_grading_of_assignments_that_involve_programming) at https://www.researchgate.net/
