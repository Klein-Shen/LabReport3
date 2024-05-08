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


<img width="443" alt="8d83bc549c83781c65f8e03419fa782" src="https://github.com/Klein-Shen/LabReport3/assets/165833763/25fba5ea-b32b-4d50-b466-4c4d079db92e">


`grep -r "happy" ./technical`
This command will recursively search for the string "happy" in all files under the `./technical` directory.


<img width="348" alt="1eacd7ea4745310b627be8ae5300767" src="https://github.com/Klein-Shen/LabReport3/assets/165833763/531f8769-651a-449c-8e15-7b99d35f41e5">


> `-i` Case-insensitive search : This option tells `grep` to perform a case-insensitive search.

`grep -i "Keyword" ./technical/*.txt`
This command will search for the string "Keyword" in the file `*.txt` under the `./technical` directory, ignoring case.


<img width="435" alt="3c200f0cac1f1b43593c62a5eb6635a" src="https://github.com/Klein-Shen/LabReport3/assets/165833763/5faf073d-5191-45f9-839b-def841ab861a">


`grep -i "LeeSin" ./technical/LOL.txt`
This command will search for the string "LeeSin" in the file `LOL.txt` under the `./technical` directory, ignoring case.


<img width="278" alt="9dd1f64172789abfc0270bef9fa052d" src="https://github.com/Klein-Shen/LabReport3/assets/165833763/14680eff-1dec-4151-a8f9-412bb3397b0c">


> `-v` Invert match : This option tells `grep` to display lines that do not match the specified pattern.

`grep -v "pattern" ./technical/*.txt`
This command will display all lines in `*.txt` under the `./technical` directory that do not contain the specified pattern.


<img width="439" alt="73225de2fa3c98f746f28d2484512f5" src="https://github.com/Klein-Shen/LabReport3/assets/165833763/b2159ed3-3ce0-4c2d-b6a5-19316350b86b">


`grep -v "unlucky" ./technical/*.txt`
This command will display all lines in `*.txt` under the `./technical` directory that do not contain the string "unlucky".


<img width="426" alt="37723bbca87276fa7c3ec84bfa27af7" src="https://github.com/Klein-Shen/LabReport3/assets/165833763/d3fe197a-23b7-4fdc-8ca7-4ccbeb09d90e">


> `-l` Display file names : This option tells `grep` to display only the names of files that contain the specified pattern, instead of the matching lines.

`grep -l "pattern" ./technical/*.txt`
This command will display the names of all `.txt` files under the `./technical` directory that contain the specified pattern.
<img width="260" alt="dda794875d878a49463c551b853b80a" src="https://github.com/Klein-Shen/LabReport3/assets/165833763/7546d7cf-644a-4d73-9767-300ab1e94516">

`grep -l "midterm" ./technical/*.txt`
This command will display the names of all `.txt` files under the `./technical` directory that contain the string "midterm".
<img width="266" alt="18d7c48c7dbc0d7eb81c618f09c4835" src="https://github.com/Klein-Shen/LabReport3/assets/165833763/b58cd60f-d5ac-4215-b81d-6852d58ef05e">

I asked ChatGPT about the usage of some command-line options related to grep, which provides the definition of grep and the meaning of some command-line options, and provides an example for each one.
