# How you found the tests with different results
I found the different results by using vimdiff. According to
the different color of the two results, I can tell which test
has different output. I go through these different color outputs manually after using vimdiff.

# Test 1

Link of the test: [test-files 22](https://github.com/nidhidhamnani/markdown-parser/blob/main/test-files/22.md)

Which is correct:
Mine is correct, since this is a link, so the output should have the link

Actual output:
![image](3.png)

Expected output:
![image](1.png)

Describe the Bug: This bug appears as the wrong one detected that there are one " " in the content. 
In this case, the method thinks this is not a link.

The code should be changed:

    String potentialLink = markdown.substring(openParen + 1, closeParen).trim();
    if(potentialLink.indexOf(" ") == -1 && potentialLink.indexOf("\n") == -1) {
        toReturn.add(potentialLink);
        currentIndex = closeParen + 1;
    }
    else {
        currentIndex = currentIndex + 1;
    }


# Test 2

Link of the test: [test-files 496](https://github.com/nidhidhamnani/markdown-parser/blob/main/test-files/496.md)

Which is correct:
The provided one is correct, since this is not a valid Link, so it should not have any output

Actual output:
![image](4.png)
The test-file/496.md one.

Expected output:
![image](2.png)

Describe the Bug: This bug appears as the wrong one detected the "\(" and "\)". 
his code grab the contends in this pair"()", and add it to toReturn ArrayList. 
However, in this case, the "()" is not in pair, since we have three "\(" but only two "\)" in this test. 
So, this is an invalid link form.
The wrong one only detected the first appeared "\(" and the first appeared "\)", and grab anything in them and detected them as a link.
In order to fix this error, we need to let the program to find the pairing "()", instead of detecting them one by one.

The code should be changed

    int openBracket = markdown.indexOf("[", currentIndex);
    int closeBracket = markdown.indexOf("]", openBracket);
    int openParen = markdown.indexOf("(", closeBracket);
    int closeParen = markdown.indexOf(")", openParen);
    if (openBracket == -1 || closeBracket == -1 || openParen == -1 || closeParen == -1) {
        return toReturn;
    }
    toReturn.add(markdown.substring(openParen + 1, closeParen));