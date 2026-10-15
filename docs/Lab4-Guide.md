# Lab 4 Guide - GitLab workflow

This file records the exact GitLab workflow used by the group, in the order required by the
lab sheet. It is kept in the repository as part of the evidence of collaboration.

## 0. Prerequisites

* Both students have Android Studio installed and can run the project on an emulator.
* Student 1 has a GitLab account.
* Replace `GROUP1` in the commands below with the real group name if a different name is used.

## 1. Student 1 - create the project and the GitLab repository (Part 1)

1. Create the Android Studio project named `GitConflictDemo-GROUP1` with the two-button screen.
2. Run it on the emulator and confirm both buttons start with the same colour (`#6750A4`).
3. In Android Studio: **Git > Create Git Repository**, then commit everything with the message
   `Create initial Android application`.
4. On GitLab: **New project > Create blank project**, name it `GitConflictDemo-GROUP1`, keep it
   private, and do **not** initialise it with a README (the repository must stay empty).
5. Connect the local repository and push:

```
git remote add origin https://gitlab.com/<username>/GitConflictDemo-GROUP1.git
git branch -M main
git push -u origin main
```

## 2. Student 1 - give Student 2 access (Part 2)

1. In GitLab: **Manage > Members > Invite members**, add Student 2 with the *Developer* role.
2. Student 2 accepts the invitation, clones the repository and runs the project:

```
git clone https://gitlab.com/<username>/GitConflictDemo-GROUP1.git
```

Both students now have the same initial version. Do not start changing the application before
this point.

## 3. Student 1 - the red feature (Part 3)

```
git switch -c feature-red-button
```

Open `app/src/main/res/layout/activity_main.xml`, change `android:backgroundTint` on
`btnPrimary` and `btnSecondary` to `#F44336`, run the app, then:

```
git add app/src/main/res/layout/activity_main.xml
git commit -m "Change button color to red"
git push -u origin feature-red-button
```

## 4. Student 2 - the green feature (Part 4)

Student 2 works from the **original** version of `main`, not from Student 1's branch:

```
git switch main
git pull
git switch -c feature-green-button
```

Change the same two `android:backgroundTint` attributes to `#4CAF50`, run the app, then:

```
git add app/src/main/res/layout/activity_main.xml
git commit -m "Change button color to green"
git push -u origin feature-green-button
```

## 5. Inspect the branches (Part 5)

```
git log --oneline --all --decorate --graph
git diff main..feature-red-button   -- app/src/main/res/layout/activity_main.xml
git diff main..feature-green-button -- app/src/main/res/layout/activity_main.xml
```

Both branches change the same two lines, to different values. That is the conflict.

## 6. Student 1 - merge request !1 (Part 6)

On GitLab: **Merge requests > New merge request**, source `feature-red-button`, target `main`,
then merge it. `main` now contains the red buttons.

Locally:

```
git switch main
git pull
git merge --no-ff feature-red-button -m "Merge branch 'feature-red-button' into main"
```

## 7. Student 2 - merge request !2 and the conflict (Part 7)

On GitLab: create a merge request from `feature-green-button` into `main`. GitLab reports
**There are merge conflicts** and offers to resolve them. That is the expected result of the
exercise; it is not a mistake.

## 8. Resolve the conflict locally (Parts 8-10)

GitLab's conflict editor can be used, but resolving locally is easier to test:

```
git switch main
git pull
git switch feature-green-button
git merge main
```

Git stops with `CONFLICT (content): Merge conflict in app/src/main/res/layout/activity_main.xml`.
The file now contains temporary markers like these (they exist only while the merge is in
progress and are never committed):

```
<<<<<<< HEAD
        android:backgroundTint="#4CAF50"
=======
        android:backgroundTint="#F44336"
>>>>>>> main
```

The group inspected both versions and decided to keep the green implementation for both
buttons. Edit the file so that both `android:backgroundTint` attributes read `#4CAF50` and no
marker lines remain, then:

```
./gradlew assembleDebug
```

Run the application on the emulator and confirm that both buttons are green and that the app
works. Only then commit the resolution:

```
git add app/src/main/res/layout/activity_main.xml
git commit -m "Resolve button color merge conflict"
git push
```

## 9. Complete the merge (Parts 11-13)

1. On GitLab, reload merge request !2. The conflict warning is gone.
2. Merge the merge request.
3. Both students update `main` and verify:

```
git switch main
git pull
./gradlew assembleDebug
```

4. Confirm that there are no conflict markers left anywhere:

```
git grep -n -e "<<<<<<<" -e ">>>>>>>" -- app/src
```

The command must produce no output.
