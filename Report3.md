# Part 1 - Bugs

This is the tests code I wrote for LinkedList.java
```
import static org.junit.Assert.*;
import org.junit.*;

public class LinkedListTests {
    @Test
    public void testPrepend(){
        Node firstHead = new Node(0, null);
        Node secondHead = new Node(1, firstHead);
        assertEquals(secondHead.next, firstHead);
        LinkedList firLinkedList = new LinkedList();
        firLinkedList.prepend(1);
        assertEquals(1, firLinkedList.root.value);
        assertEquals(null, firLinkedList.root.next);
        firLinkedList.prepend(0);
        assertEquals(0, firLinkedList.root.value);
        assertEquals(1, firLinkedList.root.next.value);
        assertEquals(null, firLinkedList.root.next.next);
    }

    @Test
    public void testAppend(){
        LinkedList firLinkedList = new LinkedList();
        assertEquals(null, firLinkedList.root);
        firLinkedList.append(1);
        assertEquals(1, firLinkedList.root.value);
        assertEquals(null, firLinkedList.root.next);
        firLinkedList.append(0);
        assertEquals(1, firLinkedList.root.value);
        assertEquals(0, firLinkedList.root.next.value);
        assertEquals(null, firLinkedList.root.next.next);
    }

    @Test
    public void testFirst(){
        LinkedList firLinkedList = new LinkedList();
        firLinkedList.append(1);
        firLinkedList.append(0);
        assertEquals(1, firLinkedList.first());
    }

    @Test
    public void testLast(){
        LinkedList firLinkedList = new LinkedList();
        firLinkedList.append(1);
        firLinkedList.append(0);
        assertEquals(0, firLinkedList.last());
    }

    @Test
    public void testToString(){
        LinkedList firLinkedList = new LinkedList();
        firLinkedList.append(1);
        firLinkedList.append(0);
        assertEquals("1 0 ", firLinkedList.toString());
        assertEquals("1 0", firLinkedList.toString()); // "1 0 " is returned. Which means "1 0" is wrong.
    }

    @Test
    public void testLength(){
        LinkedList firLinkedList = new LinkedList();
        firLinkedList.append(1);
        firLinkedList.append(0);
        assertEquals(2, firLinkedList.length());
    }
}
```

And the tests results is that
```
JUnit version 4.13.2
...E...
Time: 0.014
There was 1 failure:
1) testToString(LinkedListTests)
org.junit.ComparisonFailure: expected:<1 0[]> but was:<1 0[ ]>
        at org.junit.Assert.assertEquals(Assert.java:117)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at LinkedListTests.testToString(LinkedListTests.java:72)

FAILURES!!!
Tests run: 6,  Failures: 1
```
I made a comment beside the code.
`assertEquals("1 0 ", firLinkedList.toString());`
This code is ok. It passed
The screenshot is here.

<img width="722" alt="20240507180333" src="https://github.com/Klein-Shen/LabReport3/assets/165833763/a4c54396-a7c0-424f-96c7-7a779051be82">


They said that `In LinkedListExample at least one of the methods on LinkedList has a bug.`
I tried my best to find the bugs, but I do not know where they are.
In fact, I do not even know whether this I commented is a bug or not.

## If it is a bug.
Before code:
```
public String toString() {
        Node n = this.root;
        String s = "";
        while(n != null) {
            s += n.value + " ";
            n = n.next;
        }
        return s;
    }
```

The after code:
```
public String toString() {
        Node n = this.root;
        String s = "";
        while(n != null) {
            s += n.value;
            if (n.next == null) {
                break;
            }
            s += " ";
            n = n.next;
        }
        return s;
    }
```
Then, the output is

<img width="675" alt="20240507181251" src="https://github.com/Klein-Shen/LabReport3/assets/165833763/0e5685ec-2b3e-47ed-951b-2d380c297b4c">

And now it will return the corrected string representation of the list.

Each iteration of the original code would add an additional space, which was fine before reaching the limit, but after reaching the limit, an excess space would appear. My approach is to separate the increase of value from the increase of spaces. The increase of value remains unchanged, but I added an if statement for the increase of spaces. When the condition `n.next==null` is `true`, it jumps out of the loop directly to prevent the last increase in space.

# Part 2 - Researching Commands

I choose `grep`.

> `-r` Recursive search : This option allows `grep` to recursively search through directories for a specified pattern in files.

`grep -r "keyword" ./technical`
This command will recursively search for the string "keyword" in all files under the `./technical` directory.

`grep -r "happy" ./technical`
This command will recursively search for the string "happy" in all files under the `./technical` directory.


> `-i` Case-insensitive search : This option tells `grep` to perform a case-insensitive search.

`grep -i "Keyword" ./technical/file.txt`
This command will search for the string "Keyword" in the file `file.txt` under the `./technical` directory, ignoring case.

`grep -i "LeeSin" ./technical/LOL.txt`
This command will search for the string "LeeSin" in the file `LOL.txt` under the `./technical` directory, ignoring case.


> `-v` Invert match : This option tells `grep` to display lines that do not match the specified pattern.

`grep -v "pattern" ./technical/file.txt`
This command will display all lines in `file.txt` under the `./technical` directory that do not contain the specified pattern.

`grep -v "unlucky" ./technical/life.txt`
This command will display all lines in `file.txt` under the `./technical` directory that do not contain the string "unlucky".


> `-l` Display file names : This option tells `grep` to display only the names of files that contain the specified pattern, instead of the matching lines.

`grep -l "pattern" ./technical/*.txt`
This command will display the names of all `.txt` files under the `./technical` directory that contain the specified pattern.

`grep -l "midterm" ./technical/*.txt`
This command will display the names of all `.txt` files under the `./technical` directory that contain the string "midterm".

I asked ChatGPT about the usage of some command-line options related to grep, which provides the definition of grep and the meaning of some command-line options, and provides an example for each one.
