# how to generate requirment file

When we use python to program or do something code, we will run the code on the other compute. But the code has some dependencies package you don't know the correct version when you install.

So you can generate the `requirments.txt` file, when you run code on the other compute.


That's the function:

## No 1. On the Code Computer 

Open your compute command terminal and pip install `pipreqs` 

```shell
pip install pipreqs
```


## No 2. cd the directory where you need generate the `requirments.txt`

```shell
cd /the/directory/you/want/to/generate
pipreqs .
```

## No 3. Check your requirment file is correct

```shell
cat requirments.txt
```

The file like this
```file
requests==2.26.0
selenium==3.141.0
```

## No 4. Copy the requirment code and requirtment to the new envirment

use `pip install` to install the dependencies

```shell
pip install -r requirments.txt
```