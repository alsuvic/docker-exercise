**Task 1**: Write a Dockerfile with the content below. Substituteyou and your_email by your actual name and email, and add also a description line instead of your_description. Note that appart from installing R and some other dependencies, we add a copy of script.R within the container.

```
gedit Dockerfile
```

**Task 2**: Build a Docker image using your Dockerfile. You can use the following command, substituting <image> by the name that you want to give to your image:

```
sudo docker build -t docker-handson .
```

**Task 3**: Run a ls command using the image you just built. Which is the name of the container that this command creates?

```
sudo docker run docker-handson ls
```

**Task 4**: Run the container interactively. What's inside? Check the path of script.R

```
sudo docker run -it docker-handson

ls

ls scripts

```

**Task 5**: Run script.R --help inside the container. Hint: Note that you need to grant execution permissions to script.R and add the scripts folder to the PATH environment variable:

```
chmod +x /scripts/script.R && PATH="$PATH:/scripts"

script.R --help
```

**Task 6**: Include the following statements in your Dockerfile and build again your image. This will make script.R executable and accessible from any location within the container. Then, run again the script.R --help command within a container in a non-interactive manner (i.e. without the -i -t options), naming the container as you prefer.

```
gedit Dockerfile

sudo docker build -t docker-handson .

sudo docker run docker-handson script.R --help

```

**Task 7**: Run a container interactively from your image. Then, use R inside the container to write a file with a single column of ~1000 numbers, with the format shown below. This will be the input file for your script. Finally, run script.R and redirect the STDOUT to a file named summary.txt. Hint: If you enable the option --verbose, script.R will also print to the STDOUT the R version installed within the container. To generate the numbers, use R interactively (just type R!), e.g. the functions rnorm and write.table. Note that the output file name given to script.R should include the .pdf suffix.

```
sudo docker run -it docker-handson

R

write.table(rnorm(1000), 'input.txt', col.names=F)

q()

Rscript scripts/script.R --input input.txt --output output.pdf --verbose TRUE > summary.txt
```

**Task 8**: Repeat task 7, now mounting a volume into the folder /data_vol within the container. Substitute for the name that you prefer. Save all input and output files (input + output PDF + redirected STDOUT) for script.R in /data_vol.

```
sudo docker run -it -v volume:/data_vol docker-handson

R

write.table(rnorm(1000), 'data_vol/input.txt', col.names=F)

q()

Rscript scripts/script.R --input data_vol/input.txt --output data_vol/output.pdf --verbose TRUE > data_vol/summary.txt

ls data_vol/
```

**Task 9**: Employ a bind mount to use data/normal.txt as input for script.R within the container. Save the output files (PDF + redirected STDOUT) in the target folder. Do it in a non-interactive manner, that is, without the -i and -t options.

```
sudo docker run -v $PWD:$PWD -w $PWD docker-handson Rscript /scripts/script.R --input data/normal.txt --output output.pdf --verbose TRUE > summary.txt
```

**Task 10**: Upload your image to DockerHub.

```
sudo docker login

sudo docker tag edf42188d7ca alsuvic/docker-handson:myrepo

sudo docker push alsuvic/docker-handson
```

**Task 11**: Search DockerHub for images with the keyword ggsashimi, and pull the one built by guigolab. Then, try to reproduce the example depicted in this GitHub repo.

```
sudo docker pull guigolab/ggsashimi

git clone https://github.com/guigolab/ggsashimi.git

cd ggsashimi/

sudo docker run -w $PWD -v $PWD:$PWD guigolab/ggsashimi -b examples/input_bams.tsv -c chr10:27040584-27048100
```


## Exercise 1:

Build a Docker image for the seqClass.py script written during the Git hands-on. Make sure you can run the script within the corresponding Docker container and upload the image to DockerHub.

Firstly, we create a file to ignore the folder ggsashimi: /home/alberto/git_HandsOn/teaching/ggsashimi
```
nano .dockerignore
```

Then, we create the image.
```
sudo docker build -t docker-exercise .
```

We run the container with a volume to store the data.
```
sudo docker run -it -v volume:/data_vol docker-exercise
```

We create the data with R.
```
R
write.table(rnorm(1000), 'input.txt', col.names=F)
q()
```

We run the script.
```
Rscript scripts/script.R --input input.txt --output output.pdf --verbose TRUE > summary.txt
```

We login in our account and upload the image to Docker-Hub.
```
sudo docker login
sudo docker tag edf42188d7ca alsuvic/docker-exercise:one
sudo docker push alsuvic/docker-exercise
```
