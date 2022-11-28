## grade.sh ##
~~~
# Create your grading script here

set -e

rm -rf student-submission
git clone $1 student-submission > git.txt 2> git-err.txt
echo $? > line7.exit
cd student-submission/ > ss.txt 2> ss-err.txt
echo $? > line9.exit
FILE=ListExamples.java > f.txt 2> f-err.txt
echo $? > line11.exit

if [[ -f "$FILE" ]]
then
        echo "ListExamples.java exists!"
else
        echo "ListExamples.java! doesn't exist"
        echo "Your score is 0 out of 3!"
        exit
fi

cp ../TestListExamples.java ./ > cp.txt 2> cp-err.txt
echo $? >line22.exit
set +e

SCORE=0

javac -cp ".;../lib/*" ListExamples.java TestListExamples.java >jvc.txt 2>jvc-err.txt
echo $? >line28.exit

if [[ $? -eq 0 ]]
then
  SCORE=$(($SCORE+1))
else
  echo "Your score is" $SCORE "out of 3!"
  exit
fi

FAILED=$(java -cp ".;../lib/*" org.junit.runner.JUnitCore TestListExamples | grep -oP "(?<=,  Failures: )[0-9]+")

if [[ $? -eq 1 ]] 

then
  SCORE=$(($SCORE+2))
else
  SCORE=$(($SCORE+2-$FAILED))
fi

echo "Your score is" $SCORE "out of 3"
~~~

List-methods-nested

| line |   stdout   |    stderr    |   exit line|
|------|----------|--------------|------------|
|6     |   git.txt  |   git-err.txt|  0|                      
|8      |  ss.txt  |    ss-err.txt  |  0
10     |   f.txt     | f-err.txt    |0
22     |    cp.txt   |    cp-err.txt |  0
28     |  jvc.txt    |   jvc-err.txt | 0

![screenshot 1669649775](https://user-images.githubusercontent.com/114331111/204318736-62ddc031-0a1a-419c-9420-54b60a323e8b.jpg)

The if statement on line 15
```
if [[ -f "$FILE" ]] 
```
is true because it checks if the file is in the directory of pa1, the URL has the correct file. 

The if statement on line 32 is true because when i put "echo $?" on line 33 it prints out 0.

The if statement on line 42 is true because when i put "echo $?" on line 41, it prints out 1.

The code works when i run 
```
bash grade.sh https://github.com/ucsd-cse15l-f22/list-methods-corrected 
```
, which says
```
ListExamples.java exists!
Your score is 2 out of 3
```
but says score is 0 like the above screenshot when I run it on URL, I think the error is early exit code that doesn't allow URL to run line 33 and 43 if statements