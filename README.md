# Nextflow + Docker tutorial 

This repository contains the tutorial material for the *Parallel distributed computational workflows
with Nextflow and Docker containers* course. 

## Prerequisite

* Java 7 or 8 
* Docker engine 1.10.x (or higher) 

## Installation 

Install Nextflow by using the following command: 

```
curl -fsSL get.nextflow.io | bash
```
    
The above snippet creates the `nextflow` launcher in the current directory. 
Complete the installation moving it into a directory on your `PATH` eg: 

```
mv nextflow $HOME/bin
``` 
   
Finally, clone this repository with the following command: 

```
git clone https://github.com/nextflow-io/< ADD URL HERE>
```

## Nextflow hands-on 

During this tutorial you will implement a proof of concept of a RNA-Seq pipeline which: 

1. Indexes a trascriptome file.
2. Performs quality controls 
3. Performs quantification.
4. Create a MultiqQC report. 

## Step 1 - define the pipeline parameters 

The script `script1.nf` defines the pipeline input parameters. Run it by using the 
following command: 

```
nextflow run script1.nf
```

Try to specify a different input parameter, for example: 

```
nextflow run script1.nf --reads this/and/that
```

#### Exercise 1.1 

Modify the `script1.nf` to accept a fourth parameter named `outdir` set to s default path
which will define used as the pipeline output directory. 

#### Exercise 1.2 

Modify the `script1.nf` to print all the pipeline parameters by using a single `println` 
command and a [multiline string](https://www.nextflow.io/docs/latest/script.html#multi-line-strings)
statement.  

#### Recap 

In this step you have learned: 

1. How to define parameters in your pipeline script
2. How to pass parameters by using the command line
3. The use of `$var` and `${var}` variable placeholders 
4. How use multiline strings 


### Step 2 - Create transcriptome index file

Nextflow allows the execution of any command or user script by using a `process` definition. 

A process is defined by providing three main declarations: 
the process [inputs](https://www.nextflow.io/docs/latest/process.html#inputs), 
the process [outputs](https://www.nextflow.io/docs/latest/process.html#outputs)
and finally the command [script](https://www.nextflow.io/docs/latest/process.html#script). 

The second example adds the `index` process. Open to the to see how the process is defined. 

It takes the transcriptome file as input and creates the genome index by using the `salmon` tool. 

Note how the input declaration defines a `transcriptome` variable in the process context 
that it's is used in the command script to reference that file in the Salmon command line.

Try to run it by using the command: 

```
nextflow run script2.nf
```

The execution will fail because Salmon is not installed in the your environment. 

Add the command line option `-with-docker` to launch the execution through a Docker container
as shown below: 

```
nextflow run script.nf -with-docker
```

This time it works because it uses the Docker container `nextflow/rnaseq-nf` defined in the 
`nextflow.config` file. 

In order to avoid to add the option `-with-docker` add the following line in the `nextflow.config` file: 

```
docker.enabled = true
```

#### Exercise 2.1 

Print the output of the `index_ch` channel by using the [println](https://www.nextflow.io/docs/latest/operator.html#println)
operator (do not confuse it with the `println` statement seen previously).

#### Exercise 2.2 

Use the command `tree -a work` to see out Nextflow organises the process work directory 

 
#### Recap 

In this step you have learned: 

1. How to define a process executing a custom command
2. How process inputs are declared 
3. How process outputs are declared
4. How access the number of available CPUs
5. How print the content of a channel


### Step 3 - Collect read files by pairs

This step shows how to match *read* files into pairs, so thay can be mapped by *Salmon*. 

Edit the script `script3.nf` and add the following statement as the last line: 

```
read_pairs_ch.println()
```

Save it and execute it with the following command: 

```
nextflow run script3.nf
```

It will print an output similar to the one shown below:

```
[ggal_gut, [/../data/ggal/ggal_gut_1.fq, /../data/ggal/ggal_gut_2.fq]]
```

The above example shows how the `read_pairs_ch` channel emits tuples composed by 
two elements, where the first is the read pair prefix and the second is a list 
representing the actual files. 

Try it again specifying different read files by using a glob pattern:

```
nextflow run script3.nf --reads 'data/ggal/*_{1,2}.fq'
```


#### Exercise 3.1 

Use the [set](https://www.nextflow.io/docs/latest/operator.html#set) operator in place 
of `=` assignment to define the `read_pairs_ch` channel. 

#### Exercise 3.2 

Use the [ifEmpty](https://www.nextflow.io/docs/latest/operator.html#ifempty) operator 
to check if the `read_pairs_ch` contains at least an item. 


 
#### Recap 


### Step 3 - Perform expression quantification 


 