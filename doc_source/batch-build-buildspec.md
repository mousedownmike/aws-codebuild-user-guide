# Batch build buildspec reference<a name="batch-build-buildspec"></a>

This topic contains the buildspec reference for batch build properties\.

## batch<a name="build-spec.batch"></a>

Optional mapping\. The batch build settings for the project\.

batch/**fast\-fail**  
Optional\.     
`false`  
The default value\. All running builds will complete\.   
`true`  
All running builds will be stopped if one of the builds fails\.

By default, all batch build tasks run with the build settings such as `env` and `phases`, specified in the buildspec file\. You can override the default build settings by specifying different `env` values or a different buildspec file in the `batch/<batch-type>/buildspec` parameter\.

The contents of the `batch` property varies based on the type of batch build being specified\. The possible batch build types are:
+ [`batch/build-graph`](#build-spec.batch.build-graph)
+ [`batch/build-list`](#build-spec.batch.build-list)
+ [`batch/build-matrix`](#build-spec.batch.build-matrix)

## `batch/build-graph`<a name="build-spec.batch.build-graph"></a>

Defines a *build graph*\. A build graph defines a set of tasks that have dependencies on other tasks in the batch\. 

batch/build\-graph/**buildspec**  
Optional\. The path and file name of the buildspec file to use for this task\.

batch/build\-graph/**depend\-on**  
An array of task identifiers that this task depends on\. This task will not run until these tasks are completed\.

batch/build\-graph/**env**  
Optional\. The build environment overrides for the task\.     
batch/build\-graph/env/**compute\-type**  
The identifier of the compute type to use for the task\. See **computeType** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.  
batch/build\-graph/env/**image**  
The identifier of the image to use for the task\. See **Image identifier** in [Docker images provided by CodeBuild](build-env-ref-available.md) for possible values\.  
batch/build\-graph/env/**privileged\-mode**  
Optional\. A Boolean value that indicates whether to run the Docker daemon inside a Docker container\. Set to `true` only if the build project is used to build Docker images\. Otherwise, a build that attempts to interact with the Docker daemon fails\. The default setting is `false`\.  
batch/build\-graph/env/**type**  
The identifier of the environment type to use for the task\. See **Environment type** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.  
batch/build\-graph/env/**variables**  
The environment variables that will be present in the build environment\. See [env/variables](build-spec-ref.md#build-spec.env.variables) for more information, \.

batch/build\-graph/**identifier**  
Required\. The identifier of the task\.

batch/build\-graph/**ignore\-failure**  
Optional\. A Boolean value that indicates whether failures in the batch can be ignored\.    
`false`  
The default value\. If one build task fails, the batch build will fail\.   
`true`  
If one build task fails, the remaining build tasks will still run\. 

The following is an example of a build graph buildspec entry:

```
batch:
  fast-fail: false
  build-graph:
    - identifier: build1
      env:
        compute-type: BUILD_GENERAL1_SMALL
    - identifier: build2
      env:
        compute-type: BUILD_GENERAL1_MEDIUM
      depend-on:
        - build1
    - identifier: build3
      env:
        compute-type: BUILD_GENERAL1_LARGE
      depend-on:
        - build2
```

For more information, see [Build graph](batch-build.md#batch_build_graph)\.

## `batch/build-list`<a name="build-spec.batch.build-list"></a>

Defines a *build list*\. A build list is used to define a number of tasks that run in parallel\. 

batch/build\-list/**buildspec**  
Optional\. The path and file name of the buildspec file to use for this task\.

batch/build\-list/**env**  
Optional\. The build environment overrides for the task\.     
batch/build\-list/env/**compute\-type**  
The identifier of the compute type to use for the task\. See **computeType** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.  
batch/build\-list/env/**image**  
The identifier of the image to use for the task\. See **Image identifier** in [Docker images provided by CodeBuild](build-env-ref-available.md) for possible values\.  
batch/build\-list/env/**privileged\-mode**  
Optional\. A Boolean value that indicates whether to run the Docker daemon inside a Docker container\. Set to `true` only if the build project is used to build Docker images\. Otherwise, a build that attempts to interact with the Docker daemon fails\. The default setting is `false`\.  
batch/build\-list/env/**type**  
The identifier of the environment type to use for the task\. See **Environment type** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.  
batch/build\-list/env/**variables**  
The environment variables that will be present in the build environment\. See [env/variables](build-spec-ref.md#build-spec.env.variables) for more information, \.

batch/build\-list/**identifier**  
Optional\. The identifier of the task\.

batch/build\-list/**ignore\-failure**  
Optional\. A Boolean value that indicates whether failures in the batch can be ignored\.    
`false`  
The default value\. If one build task fails, the batch build will fail\.   
`true`  
If one build task fails, the remaining build tasks will still run\. 

The following is an example of a build list buildspec entry:

```
batch:
  fast-fail: false
  build-list:
    ignore-failure: true
      - identifier: linux_small
        env:
          compute-type: BUILD_GENERAL1_SMALL
      - identifier: windows_medium
        env:
          type: WINDOWS_SERVER_2019_CONTAINER
          image: aws/codebuild/windows-base:2019-1.0
          compute-type: BUILD_GENERAL1_MEDIUM
```

For more information, see [Build list](batch-build.md#batch_build_list)\.

## `batch/build-matrix`<a name="build-spec.batch.build-matrix"></a>

Defines a *build matrix*\. A build matrix is used to define tasks that will run in parallel with different environments\. CodeBuild creates a separate build for each possible environment configuration\. 

batch/build\-matrix/**static**  
The static properties apply to all build tasks\.    
batch/build\-matrix/static/**ignore\-failure**  
Optional\. A Boolean value that indicates whether failures in the batch can be ignored\.    
`false`  
The default value\. If one build task fails, the batch build will fail\.   
`true`  
If one build task fails, the remaining build tasks will still run\.   
batch/build\-matrix/static/**env**  
Optional\. The build environment overrides for the task\.     
batch/build\-matrix/static/env/**privileged\-mode**  
Optional\. A Boolean value that indicates whether to run the Docker daemon inside a Docker container\. Set to `true` only if the build project is used to build Docker images\. Otherwise, a build that attempts to interact with the Docker daemon fails\. The default setting is `false`\.  
batch/build\-matrix/static/env/**type**  
Optional\. The identifier of the environment type to use for the task\. See **Environment Type** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.

batch/build\-matrix/**dynamic**  
The dynamic properties define the build matrix\.    
batch/build\-matrix/dynamic/**buildspec**  
Optional\. The path and file name of the buildspec file to use for this task\.  
batch/build\-matrix/dynamic/**env**  
Optional\. The build environment overrides for the task\.     
batch/build\-matrix/dynamic/env/**compute\-type**  
The identifier of the compute type to use for the task\. See **computeType** in [Build environment compute types](build-env-ref-compute-types.md) for possible values\.  
batch/build\-matrix/dynamic/env/**image**  
Optional\. The identifier of the image to use for the task\. See **Image identifier** in [Docker images provided by CodeBuild](build-env-ref-available.md) for possible values\.  
batch/build\-matrix/dynamic/env/**variables**  
The environment variables that will be present in the build environment\. See [env/variables](build-spec-ref.md#build-spec.env.variables) for more information, \.

The following is an example of a build matrix buildspec entry:

```
batch:
  build-matrix:
    static:
      ignore-failure: false
      env:
        type: LINUX_CONTAINER
        privileged-mode: true
    dynamic:
      env:
        image:
          - aws/codebuild/amazonlinux2-x86_64-standard:3.0
          - aws/codebuild/windows-base:2019-1.0
        variables:
          MY_VAR:
            - VALUE1
            - VALUE2
            - VALUE3
```

For more information, see [Build matrix](batch-build.md#batch_build_matrix)\.