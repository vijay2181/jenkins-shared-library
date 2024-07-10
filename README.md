# jenkins-shared-library

- we can develop reusable code using Jenkins shared library in pipeline jobs.

![image](https://github.com/vijay2181/jenkins-shared-library/assets/66196388/431cf3d4-c79f-4c6f-b8b4-df7d165be699)

- var folder will have main grovvy script where we will put our resuable function
- src has utilities
- static configuration data can be put in resources folder
- we will inplement shared library using vars folder
- all 3 folders are not mandatory, src or vars folders are mandatory
- var/greet.groovy we have created a function definition(call) with parameter(name) -> call(name)
- greet file name will be referenced in pipeline to call call function
- when we want to call this function -> call -> from jenkins pipeline, we need to use greet as a step name and we will pass the name there
- inorder to use jenkins shared library, we need to configure it.
- goto -> manage jenkins -> configure system -> Global Pipeline Libraries -> we need to put shared library repo details here

```
vars/greet.groovy

def call(name){
  echo "Hi ${name}, How are you?"
}

- here greet.grrovy file will call by deafult call function
  call can be of any name but by default name is call
```

```
Name: sharedlib-demo
Default value: main(branch)
Retrival Method: Modern SCM
Source Code Mnagement: Git(https://github.com/vijay2181/jenkins-shared-library.git)
save
```

```
step1: create a project in github and follow standard structure and
       write one groovy script function where you want to reuse in your jenkins pipeline
step2: configure that shared library(sharedlib-demo) under manage jenkins section
step3: use that shared library name in whatever the project you need

syntax to import sharedlibrary: @Library('sharedlib-demo') -
```

- the groovy file name inside the shared library is -> greet -> it takes one argument

```
pipeline {
  agent any
  stages {
    stage('Demo') {
      steps {
        greet("vijay")
      }
    }
  }
}

output: Hi vijay, How are you?

- if you want to use this shared library in one more job,
  then you need to import it and call multiple times
```

- multiple functions implementation

```
vars/calculator.groovy

def add(x,y){
  echo "Sum of ${x} and ${y} is ${x+y}"
}

def mul(x,y){
  echo "Multiplication of ${x} and ${y} is ${x*y}"
}
```

```
pipeline {
  agent any
  stages {
    stage('Demo') {
      steps {
        greet("vijay")
        greet("kumar")
        script{
          calculator.add(5,10)
          calculator.mul(20,30)
          }
      }
    }
  }
}


- if we want to use groovy syntaxes directly inside jenkins pipeline,
  then we need to wrap them inside script block
```
