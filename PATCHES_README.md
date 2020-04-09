# zf1 repo with patches managed by Puppet

## Supports legacy apps that use zf1 with modern PHP incompatibilities

## Usage (with examples)

## checkout the desired release

`git checkout release-1.12.1`

## create patch branch

`git checkout -b patch-1.12.1-php7.2-compatible`

## create a patch

### fix something

`vi library/Zend/Db/Table/Abstract.php`

### commit

`git add library/Zend`

`git commit`

### make the patch

Create a patch against a specific release branch.

The patch will be saved in directory "patches" with name formated "YYYYMMDDHHMMSS\_something\_descriptive.patch"

`git format-patch release-1.12.1 --stdout > patches/20200409092900_fix_count_generates_warning.patch`

### commit the patch

`git add patches`

`git commit`

## apply a patch manually

### check the status of the patch

`cd /usr/share/zf1`

`git apply --stat - < /pathto/20200409092900_fix_count_generates_warning.patch`

### check whether patch will apply cleanly, should be silent if all is ok

`git apply --check - < /pathto/20200409092900_fix_count_generates_warning.patch -p 2`

### apply patch

`git apply - < /pathto/20200409092900_fix_count_generates_warning.patch -p 2`

## apply a patch with puppet

Save the patchfile in the Puppet a repo's (default zf1) files directory


`class { '::zf1::patch': 
   package_ver => '1.12.1',
   srcrepo     => 'zf1',
   patches     => ['20200409092900_fix_count_generates_warning'],
}`