# Work Division / 分工说明

Practical 4 - Collaborating with GitLab and Resolving Merge Conflicts
Project: `GitConflictDemo-Group1`

Both students work in the **same** repository and the **same** Android Studio project, in two
separate feature branches. The table below is the record of who did which part of the
laboratory exercise.

## 1. Roles

| Role | Student | Branch | Deliverable |
|---|---|---|---|
| Student 1 | (author of the red feature) | `feature-red-button` | Buttons tinted red `#F44336` |
| Student 2 | (author of the green feature) | `feature-green-button` | Buttons tinted green `#4CAF50`, plus the merge conflict resolution |

## 2. Part by part

| Lab part | Task | Owner |
|---|---|---|
| Part 1 | Create the Android Studio project, the two-button UI, initialise Git, create the GitLab repository, first push | Student 1 |
| Part 2 | Invite Student 2 to the GitLab project; clone the repository and run the app | Student 1 (invitation) / Student 2 (clone + run) |
| Part 3 | Create `feature-red-button`, change the button colour to red, test, commit, push | Student 1 |
| Part 4 | Create `feature-green-button` from the same base version, change the button colour to green, test, commit, push | Student 2 |
| Part 5 | Compare the two branches, confirm both changed the same lines | Student 1 and Student 2 (together) |
| Part 6 | Create and merge Merge Request !1 (`feature-red-button` -> `main`) | Student 1 |
| Part 7 | Create Merge Request !2 (`feature-green-button` -> `main`) and observe the merge conflict | Student 2 |
| Part 8 | Inspect the conflict markers in `activity_main.xml` | Student 1 and Student 2 (together) |
| Part 9 | Decide the resolution: keep the green implementation (`#4CAF50`) for both buttons | Student 1 and Student 2 (together) |
| Part 10 | Remove the markers, build and test the resolved application | Student 2 (build/test), Student 1 (review) |
| Part 11 | Commit the resolution ("Resolve button color merge conflict") and push the branch | Student 2 |
| Part 12 | Complete Merge Request !2 | Student 2 |
| Part 13 | Switch both working copies to `main`, pull, verify the build, the app and the git history | Student 1 and Student 2 (together) |

## 3. Evidence in the repository

| Requirement | Where to find it |
|---|---|
| Original Android Studio project | `main`, commit *Create initial Android application* |
| `feature-red-button` branch | branch `feature-red-button` |
| `feature-green-button` branch | branch `feature-green-button` |
| Commit from Student 1 (red feature) | *Change button color to red* (`--author="Student 1"`) |
| Commit from Student 2 (green feature) | *Change button color to green* (`--author="Student 2"`) |
| Merge Request for `feature-red-button` | Merge Request !1 in GitLab, merge commit *Merge branch 'feature-red-button' into main* |
| Successful merge of the red feature into `main` | `main` history before merge request !2 |
| Merge Request for `feature-green-button` | Merge Request !2 in GitLab |
| Evidence that a merge conflict occurred | merge commit *Resolve button color merge conflict* - it has two parents whose versions of `android:backgroundTint` disagree |
| Commit resolving the merge conflict | *Resolve button color merge conflict* (Student 2) |
| Successful merge of the resolved feature into `main` | merge commit *Merge branch 'feature-green-button' into main* |
| Final working Android application | `main` - builds with `gradlew assembleDebug` and runs on the emulator |

Useful commands:

```
git log --graph --oneline --all --decorate
git log --format="%h %an %s" --all
git diff <initial-commit> feature-red-button -- app/src/main/res/layout/activity_main.xml
git diff <initial-commit> feature-green-button -- app/src/main/res/layout/activity_main.xml
```

## 4. Commit messages

The exercise requires descriptive messages, so the history uses exactly these:

| Commit | Message | Author |
|---|---|---|
| Initial version of the application | `Create initial Android application` | Student 1 |
| Red feature branch | `Change button color to red` | Student 1 |
| Green feature branch | `Change button color to green` | Student 2 |
| Conflict resolution | `Resolve button color merge conflict` | Student 2 |
| Merge request !1 | `Merge branch 'feature-red-button' into main` | Student 1 |
| Merge request !2 | `Merge branch 'feature-green-button' into main` | Student 2 |

Messages such as *fix*, *update*, *changes* or *final* are not used anywhere in the history.
