# debian_prokka_panx_antismash_biodocker
Guide to installing Docker, building Image, and running Container
Installation latest Docker (docker-engine 1.12.5-0~ubuntu-xenial) on Ubuntu Xenial 16.04 (LTS):
1. Copy the following commands for quickly & easily installing via latest docker-engine (Ubuntu, Debian, Raspbian, Fedora, Centos, Redhat, Suse, Oracle, Linux et al., all applicable):
$ curl -sSL https://get.docker.com/ | bash -x
or:
$ wget -qO- https://get.docker.com/ | bash -x
2. Type the following commands at your shell prompt. If it outputs the Docker version, your installation was successful: 
$ docker version

Installing latest Docker on Windows 10 Enterprise:
The current version of Docker for Windows runs on 64-bit Windows 10 Pro, Enterprise, and Education editions.
1. Download Docker InstallDocker.msi
(https://download.docker.com/win/stable/InstallDocker.msi)
2. Install Docker (1.12.0-rc2) for Windows
3. Double-click InstallDocker.msi to run the installer. Follow the install wizard to accept the license, authorize the installer, and proceed with the installation.
4. Type the following commands at your shell prompt (cmd.exe or PowerShell). If it outputs the Docker version, your installation was successful.
$ docker version

On your host, type the following command lines to log into a BGDMdocker Container: 
$ git clone https://github.com/cgwyx/debian_prokka_panx_antismash_
biodocker.git
or: download “debian_prokka_panx_antismash_biodocker-master.zip” file
$ unzip debian_prokka_panx_antismash_biodocker-master.zip 

Building images of workflow:
$ cd ./debian_prokka_panx_antismash_biodocker/prokka_panx_antismash_dockerfile
$ sudo docker build -t BGDMdocker:latest .

Running a Container from image of BGDMdocker:latest: 
$ sudo docker run -it --rm –v home:home –p 8000:8000 --name=BGDM-docker BGDMdocker:latest
If you use the “-v home:home” parameter, Docker will mount the local folder /home into the Container under /home, storing all of your data in one directory in the home folder of the host operating system; then, you may access directories of home from inside the container.
Guide for workflow usage (running inside Container)
Running Prokka genome annotation in Container in interaction patterns (if you have your own genome sequences, you need this step to generate “*.gbff” annotation files):
1. Check out help documentation and command parameters:
$ prokka –help
2. Copy the following commands to run the analysis for genome annotation of Ba_xx strains from the command-line interface of container (for boldface text, please enter your data): 
$ prokka --kingdom Bacteria --gcode 11 --genus Bacillus \
--species Amyloliquefaciens \
--strain Ba_xx --locustag Ba_xx --prefix Ba_xx --rfam \
--rawproduct --outdir /home/manager/PRJNA291327 \
/home/manager/Ba_xx.fasta
“Ba_xx.fasta” is the genome sequence, “PRJNA291327” is the output folder of results.

Running panX analysis on pan-genome in Container in interaction patterns:
PanX starts with a set of annotated sequences files, *.gbff (.gbk) (e.g., NCBI RefSeq or GenBank), from a bacterial species genome. 
1. Check out help documentation and command parameters of panX:
$ sh run.sh
2. Copy the following commands to run the analysis of the pan-genome of 44 B. amyloliquefaciens strains from the command-line interface of Container (for boldface text, please enter your data). If using your own GenBank files (or if you have downloaded these files), parameter “2” can be skipped when running “run-pipeline.py”. For detailed parameters see here:
$ cd /pan-genome-analysis
$ python ./scripts/run-pipeline.py -fn ./data/B_amy -sl B_amy-
RefSeq.txt -st 1 3 4 5 6 7 8 9 10 11
*.gbff (GenBank files) and B_amy-RefSeq.txt (accession list for strains) should reside in “./data/B_amy” folder. The result will also be output to the “./data/B_amy” folder.
3. Visualization of the pan-genome of 44 B. amyloliquefaciens strains (run in container):
$ python link-to-server.py B_amy 
$ add-new-pages-repo.sh B_amy
$ gulp
Then open http://localhost:8000/B_amy with web browser, you can access the visualization of the pan-genome immediately.
4. Create a new Image for saving changes in Container data (running in host):
$ sudo docker commit <ID of Container > <name of new Image >

Running antiSAMSH to search for gene clusters in every strain in Container in interaction patterns:
1. Check out help documentation and command parameters:
$ run_antismash.py --help
2. Copy the following commands to run the analysis for biosynthetic gene clusters of Y2 strain from the command-line interface of container (Y2.gbff) (for boldface text, please enter your data):
$ cd /antismash-3.0.5/antismash/
$ run_antismash.py /home/manager/input/Y2.gbff \
--outputfolder /home/manager/output/Y2 _out \
--dbgclusterblast ./generic_modules/clusterblast \
--pfamdir ./generic_modules/fullhmmer --input-type nucl --knownclusterblast \
--clusterblast --subclusterblast --inclusive --full-hmmer --smcogs --verbose --asf \
--borderpredict 
*.gbff (GenBank files) should reside in “input” folder. “Y2 _out” is the output folder for results.
Recommendation for building workflow using standalone Dockerfile:
In order to meet the needs of different users, we also provide a standalone Dockerfile for Prokka, panX, and antiMASH. You can build images and run Container separately. 
$ git clone https://github.com/cgwyx/debian_prokka_panx_antismash_
docker.git
Or: download “.zip”file
$ unzip debian_prokka_panx_antismash_biodocker-master.zip

Building images of Prokka standalone:
$ cd ./debian_prokka_panx_antismash_docker/debian_prokka_standalone_dockerfile
$ sudo docker build -t debian_prokka: latest .
1. Run a Container from image of debian_prokka:
$ sudo docker run -it --rm -v home:home --name Prokka-latest prokka:latest
2. Copy the following commands to run the analysis for genome annotation of Ba_xx strains from the command-line interface of container (for boldface text, please replace with your own data if applicable): 
$ prokka --kingdom Bacteria --gcode 11 --genus Bacillus \
--species Amyloliquefaciens \
--strain Ba_xx --locustag Ba_xx --prefix Ba_xx --rfam \
--rawproduct --outdir /home/manager/PRJNA291327 \
/home/manager/Ba_xx.fasta
“Ba_xx.fasta” is the sequence of the genome; “PRJNA291327” is the output folder of results.

Building images of panX standalone:
$ cd ./debian_prokka_panx_antismash_docker/debian_panx_standalone_dockerfile
$ sudo docker build -t debian_panx:latest .
1. Run a container from image of debian_panx
$ sudo docker run -it --rm -v home:home --name debian_panx debian_panx:latest
2. Copy the following commands to run the analysis for the pan-genome of 44 B. amyloliquefaciens strains from the command-line interface of container (for boldface text, please replace with your own data if applicable):
$ cd /pan-genome-analysis
$ python ./scripts/run-pipeline.py -fn ./data/B_amy -sl B_amy-RefSeq.txt -st 1 3 4 5 6 7 8 9 10 11
*.gbff (GenBank files) and B_amy-RefSeq.txt (accession list for strains) should be in the “./data/B_amy” folder; output results will be also in “./data/B_amy” folder. 
3. Visualize the pan-genome of 44 B. amyloliquefaciens strains (run in container):
$ python link-to-server.py B_amy 
$ add-new-pages-repo.sh B_amy
$ gulp
Then open http://localhost:8000/B_amy with web browser, you can access the visualization of the pan-genome immediately.
4. Create a new Image for saving changes in Container data (running in host):
$ sudo docker commit <ID of container > <name of new images >

Building images of antiSAMSH standalone:
$ cd ./debian_prokka_panx_antismash_docker/debian_antismash_standalone_dockerf-ile
$ sudo docker build -t debian_antismas:latest . 
1. Run a container from image of debian_antismas
$ sudo docker run -it --rm -v home:home --name antiSAMSH debian_antismas:latest
2. Copy the following commands to run the analysis of biosynthetic gene clusters of Y2 strain from the command-line interface of container (Y2.gbff) (for boldface text, please replace with your own data if applicable):
$ cd /antismash-3.0.5/antismash/
$ run_antismash.py /home/manager/input/Y2.gbff \
--outputfolder /home/manager/output/Y2 _out \
--dbgclusterblast ./generic_modules/clusterblast \
--pfamdir ./generic_modules/fullhmmer --input-type nucl --knownclusterblast \
--clusterblast --subclusterblast --inclusive --full-hmmer --smcogs --verbose --asf \
--borderpredict 
*.gbff (GenBank files) reside in “input”folder; “Y2 _out” is the output folder for results.

Tip: How can I download all “genomic.gbff.gz” of specified species from RefSeq or GenBank databases?
Downloading all “*genomic.gbff.gz” of specified species from RefSeq or GenBank databases (replace boldface text with your species if applicable):
1. Installing script on your host
$ wget 
ftp://ftp.ncbi.nlm.nih.gov/entrez/entrezdirect/versions/current/edirect.zip
$ unzip -u -q edirect.zip
$ export PATH=$PATH:$HOME/edirect
$ ./edirect/setup.sh
2. Download “*genomic.gbff.gz” of all strains of Bacillus amyloliquefaciens from GenBank
$ esearch -db assembly -query "Bacillus amyloliquefaciens [ORGN]" | efetch -format docsum | xtract -pattern "DocumentSummary" -element FtpPath_GenBank | sed 's/$/\/*genomic.gbff.gz/' |xargs wget -c -nd;sleep 3s;
Or:
3. Download “*genomic.gbff.gz” of all Bacillus amyloliquefaciens strains from RefSeq
$ esearch -db assembly -query "Bacillus amyloliquefaciens[ORGN]" | efetch -format docsum | xtract -pattern "DocumentSummary" -element FtpPath_RefSeq | sed 's/$/\/*genomic.gbff.gz/' |xargs wget -c -nd;sleep 3s;
