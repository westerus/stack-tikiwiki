# Introduction to pipelines and builds

>**Note:**
Introduced in GitLab 8.8.

## Pipelines

A pipeline is a group of [builds][] that get executed in [stages][](batches).
All of the builds in a stage are executed in parallel (if there are enough
concurrent [Runners]), and if they all succeed, the pipeline moves on to the
next stage. If one of the builds fails, the next stage is not (usually)
executed.

![Pipelines example](img/pipelines.png)

## Builds

Builds are individual runs of [jobs]. Not to be confused with a `build` job or
`build` stage.

## Defining pipelines

Pipelines are defined in `.gitlab-ci.yml` by specifying [jobs] that run in
[stages].

See full [documentation](yaml/README.md#jobs).

## Seeing pipeline status

You can find the current and historical pipeline runs under **Pipelines** for
your project.

## Seeing build status

Clicking on a pipeline will show the builds that were run for that pipeline.
Clicking on an individual build will show you its build trace, and allow you to
cancel the build, retry it,  or erase the build trace.

## How the pipeline duration is calculated

Total running time for a given pipeline would exclude retries and pending
(queue) time. We could reduce this problem down to finding the union of
periods.

So each job would be represented as a `Period`, which consists of
`Period#first` as when the job started and `Period#last` as when the
job was finished. A simple example here would be:

* A (1, 3)
* B (2, 4)
* C (6, 7)

Here A begins from 1, and ends to 3. B begins from 2, and ends to 4.
C begins from 6, and ends to 7. Visually it could be viewed as:

```
0  1  2  3  4  5  6  7
   AAAAAAA
      BBBBBBB
                  CCCC
```

The union of A, B, and C would be (1, 4) and (6, 7), therefore the
total running time should be:

```
(4 - 1) + (7 - 6) => 4
```

## Badges

Build status and test coverage report badges are available. You can find their
respective link in the [Pipelines settings] page.

[builds]: #builds
[jobs]: yaml/README.md#jobs
[stages]: yaml/README.md#stages
[runners]: runners/README.html
[pipelines settings]: ../user/project/pipelines/settings.md
