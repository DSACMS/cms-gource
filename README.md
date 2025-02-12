# cms-gource

[![Screenshot of Gource.io visualization of Developer.CMS.gov source code repositories](https://img.youtube.com/vi/nd4ORHxgvAY/0.jpg "https://www.youtube.com/watch?v=nd4ORHxgvAY")](https://www.youtube.com/watch?v=nd4ORHxgvAY)

This repository contains configuration scripts, log files, and graphic assets
used to generate a source code repository visualization using [Gource](https://gource.io).

This work was presented at the
[SCaLE20x](https://www.socallinuxexpo.org/scale/20x) conference in March of
2023 in a talk entitled [Repodiving into Open Source at
CMS.gov](https://www.socallinuxexpo.org/scale/20x/presentations/repodiving-open-source-cmsgov),
with the recording uploaded to the [conference YouTube
Channel](https://youtu.be/AypgQch2Qpk).

For more information on how to use [CMS.gov](https://cms.gov)'s collection of
APIs, datasets, frameworks, and style guides to develop applications that help
people get the services and benefits they rely on, visit
[developer.cms.gov](https://developer.cms.gov).

## Usage
1. install dependencies
2. create repos directory
3. clone repos
4. generate logs
5. insert names
6. colorize logs
7. Concatenate and sort logs
8. run gource 
9. export video

### Install Dependencies 

The author used the open source [Homebrew](https://brew.sh) project to install
the gource and ffmpeg tools on a Mac. 

    brew install gource
    brew install ffmpeg


Gource and ffmpeg can also be installed directly from source on
[GitHub.com](https://github.com/acaudwell/Gource) and
[ffmpeg.org](https://ffmpeg.org/download.html#get-sources) or via most Linux
distribution package managers.

Author used standard commandline tools such as the [Vim text
editor](https://www.vim.org) to add hexadecimal color codes to the end of each
line in the log to colorize the output, and the [sed stream
editor](https://www.gnu.org/software/sed/) to insert parent names into logs.
Most text manipulation tools capable of regular expressions or find-and-replace
functions should work to achieve the same results.

### Create repos directory

    mkdir repos 
    cd repos


### Clone Repos

    git clone https://github.com/CMSgov/ab2d.git
    git clone https://github.com/CMSgov/bcda-app.git
    git clone https://github.com/CMSgov/beneficiary-fhir-data.git
    git clone https://github.com/CMSgov/bluebutton-web-server.git
    git clone https://github.com/CMSgov/dpc-app.git
    git clone https://github.com/CMSgov/design-system.git


### Generate Logs

    gource --output-custom-log log1.txt repos/ab2d
    gource --output-custom-log log2.txt repos/bcda-app
    gource --output-custom-log log3.txt repos/beneficiary-fhir-data
    gource --output-custom-log log4.txt repos/bluebutton-web-server
    gource --output-custom-log log5.txt repos/dpc-app
    gource --output-custom-log log6.txt repos/design-system

### Insert Names

    sed -i -r -E "s#(.+)\|#\1|/ab2d#" log1.txt
    sed -i -r -E "s#(.+)\|#\1|/bcda#" log2.txt
    sed -i -r -E "s#(.+)\|#\1|/beneficiary-fhir-data#" log3.txt
    sed -i -r -E "s#(.+)\|#\1|/blue-button#" log4.txt
    sed -i -r -E "s#(.+)\|#\1|/dpc#" log5.txt
    sed -i -r -E "s#(.+)\|#\1|/design-system#" log6.txt

### Colorize Logs (using vim)

    vim log1.txt
    :%norm A|F0F0F0

    vim log2.txt
    :%norm A|136A5D

    ...


### Concatenate and Sort Logs

    APIs only
    cat log1.txt log2.txt log3.txt log4.txt log5.txt | sort -n > cmsapis.log

    APIs + design-system
    cat log1.txt log2.txt log3.txt log4.txt log5.txt log6.txt | sort -n > cmsapis-plusdesign.log

    APIs + QPP quality tools
    cat log1.txt log2.txt log3.txt log4.txt log5.txt log7.txt log8.txt | sort -n > cmsapis-plusqpp.log



### Run Gource Visualization

    APIs Only
    time gource --load-config cms.conf -f -1920x1080 cmsapis.log 

    API + design-system
    time gource --load-config cms.conf -f -1920x1080 cmsapis-plusdesign.log 

(last output: 131.51s user 6.60s system 33% cpu 6:51.33 total)

### Export Video (.webm or .mp4)

    gource --load-config cms.conf -1920x1080 cmsapis.log -f -o - | ffmpeg -y -r 60 -f image2pipe -vcodec ppm -i - -vcodec libvpx -b 10000K cms-gource.webm

    gource --load-config cms.conf -1920x1080 cmsapis.log -f -o - | ffmpeg -y -r 60 -f image2pipe -vcodec ppm -i - -vcodec libx264 -preset ultrafast -pix_fmt yuv420p -crf 1 -threads 0 -bf 0 cms-gource.mp4


## LICENSE
As a work of the United States Government, this project is in the
public domain within the United States.

Additionally, we waive copyright and related rights in the work
worldwide through the CC0 1.0 Universal public domain dedication.

### CC0 1.0 Universal Summary
This is a human-readable summary of the [Legal Code (read the full
text)](https://creativecommons.org/publicdomain/zero/1.0/legalcode).

### No Copyright
The person who associated a work with this deed has dedicated the work to
the public domain by waiving all of his or her rights to the work worldwide
under copyright law, including all related and neighboring rights, to the
extent allowed by law.

You can copy, modify, distribute and perform the work, even for commercial
purposes, all without asking permission.

### Other Information
In no way are the patent or trademark rights of any person affected by CC0,
nor are the rights that other persons may have in the work or in how the
work is used, such as publicity or privacy rights.

Unless expressly stated otherwise, the person who associated a work with
this deed makes no warranties about the work, and disclaims liability for
all uses of the work, to the fullest extent permitted by applicable law.
When using or citing the work, you should not imply endorsement by the
author or the affirmer.

## Acknowledgments

CMS would like to thank [General Services Administration](https://gsa.gov)
(GSA)’s [18F](https://18f.gsa.gov) team, the [Consumer Financial Protection
Bureau](https://cfpb.gov) (CFPB), and the [Office of Management and
Budget](https://www.whitehouse.gov/omb/) (OMB) for their inspirational work in
the use of Free/Open Source Software in the Federal Government.

Author would like to give special thanks Andrew Caudwell and the
[https://Gource.io](https://Gource.io) Project contributors. Source code
repository available at
[https://github.com/acaudwell/Gource](https://github.com/acaudwell/Gource)
 
 ## About the Project 
<!-- This should be a longer-form description of the project. It can include history, background, details, problem statements, links to design documents or other supporting materials, or any other information/context that a user or contributor might be interested in. --> 
 
 ## Core Team 
An up-to-date list of core team members can be found in [MAINTAINERS.md](MAINTAINERS.md). At this time, the project is still building the core team and defining roles and responsibilities. We are eagerly seeking individuals who would like to join the community and help us define and fill these roles. 
 
 ## Documentation Index 
<!-- TODO: This is a like a 'table of contents' for your documentation. Tier 0/1 projects with simple README.md files without many sections may or may not need this, but it is still extremely helpful to provide 'bookmark' or 'anchor' links to specific sections of your file to be referenced in tickets, docs, or other communication channels. --> 
 **{list of .md at top directory and descriptions}** 
 
 ## Repository Structure 
<!-- TODO: Using the 'tree -d' command can be a helpful way to generate this information, but, be sure to update it as the project evolves and changes over time. --> 
 <!--TREE START--><!--TREE END--> 
 **{list directories and descriptions}** 
 
 ## Development and Software Delivery Lifecycle 
The following guide is for members of the project team who have access to the repository as well as code contributors. The main difference between internal and external contributions is that external contributors will need to fork the project and will not be able to merge their own pull requests. For more information on contributing, see: [CONTRIBUTING.md](./CONTRIBUTING.md). 
 
 ## Local Development 
<!--- TODO - with example below: 
This project is monorepo with several apps. Please see the [api](./api/README.md) and [frontend](./frontend/README.md) READMEs for information on spinning up those projects locally. Also see the project [documentation](./documentation) for more info. --> 
 
 ## Coding Style and Linters 
<!-- TODO - Add the repo's linting and code style guidelines --> 
 Each application has its own linting and testing guidelines. Lint and code tests are run on each commit, so linters and tests should be run locally before commiting. 
 
 ## Branching Model 
 <!-- TODO - with example below:
 This project follows [trunk-based development](https://trunkbaseddevelopment.com/), which means:

* Make small changes in [short-lived feature branches](https://trunkbaseddevelopment.com/short-lived-feature-branches/) and merge to `main` frequently.
* Be open to submitting multiple small pull requests for a single ticket (i.e. reference the same ticket across multiple pull requests).
* Treat each change you merge to `main` as immediately deployable to production. Do not merge changes that depend on subsequent changes you plan to make, even if you plan to make those changes shortly.
* Ticket any unfinished or partially finished work.
* Tests should be written for changes introduced, and adhere to the text percentage threshold determined by the project.

This project uses **continuous deployment** using [Github Actions](https://github.com/features/actions) which is configured in the [./github/workflows](.github/workflows) directory.

Pull-requests are merged to `main` and the changes are immediately deployed to the development environment. Releases are created to push changes to production.--> 
 
 ## Contributing 
Thank you for considering contributing to an Open Source project of the US Government! For more information about our contribution guidelines, see [CONTRIBUTING.md](CONTRIBUTING.md). 
 
 ## Codeowners 
The contents of this repository are managed by **{responsible organization(s)}**. Those responsible for the code and documentation in this repository can be found in [CODEOWNERS.md](CODEOWNERS.md). 
 
 ## Community 
The {name_of_project_here} team is taking a community-first and open source approach to the product development of this tool. We believe government software should be made in the open and be built and licensed such that anyone can download the code, run it themselves without paying money to third parties or using proprietary software, and use it as they will.

We know that we can learn from a wide variety of communities, including those who will use or will be impacted by the tool, who are experts in technology, or who have experience with similar technologies deployed in other spaces. We are dedicated to creating forums for continuous conversation and feedback to help shape the design and development of the tool.

We also recognize capacity building as a key part of involving a diverse open source community. We are doing our best to use accessible language, provide technical and process documents, and offer support to community members with a wide variety of backgrounds and skillsets. 
 
 ## Community Guidelines 
Principles and guidelines for participating in our open source community are can be found in [COMMUNITY_GUIDELINES.md](COMMUNITY_GUIDELINES.md). Please read them before joining or starting a conversation in this repo or one of the channels listed below. All community members and participants are expected to adhere to the community guidelines and code of conduct when participating in community spaces including: code repositories, communication channels and venues, and events. 
 
 ## Feedback 
If you have ideas for how we can improve or add to our capacity building efforts and methods for welcoming people into our community, please let us know at **{contact_email}**. If you would like to comment on the tool itself, please let us know by filing an **issue on our GitHub repository.** 
 
 ## Policies 
 
 ### Open Source Policy 
We adhere to the [CMS Open Source Policy](https://github.com/CMSGov/cms-open-source-policy). If you have any questions, just [shoot us an email](mailto:opensource@cms.hhs.gov). 
 
 ### Security and Responsible Disclosure Policy 
*Submit a vulnerability:* Vulnerability reports can be submitted through [Bugcrowd](https://bugcrowd.com/cms-vdp). Reports may be submitted anonymously. If you share contact information, we will acknowledge receipt of your report within 3 business days.
For more information about our Security, Vulnerability, and Responsible Disclosure Policies, see [SECURITY.md](SECURITY.md). 
 
 ### Software Bill of Materials (SBOM) 
A Software Bill of Materials (SBOM) is a formal record containing the details and supply chain relationships of various components used in building software.
In the spirit of [Executive Order 14028 - Improving the Nation's Cyber Security](https://www.gsa.gov/technology/it-contract-vehicles-and-purchasing-programs/information-technology-category/it-security/executive-order-14028), a SBOM for this repository is provided here: https://github.com/{repo_org}/{repo_name}/network/dependencies.
For more information and resources about SBOMs, visit: https://www.cisa.gov/sbom.