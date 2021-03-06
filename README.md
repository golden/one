<a name=top>
<h1 align=center>GOLD: the Gawk Object Layer</h1>
<p  align=center>
<a href="http://github.com/golden/one/master/blob/README.md#top">home</a> :: 
<a href="http://github.com/golden/issues">issues</a> 
</p>

Q1: Why use GOLD?   
A1: GAWK is a very portable and succinct scripting language. 
But is 
has some shortcomings: e.g. weak encapsulation and packaging support, no IDE or unit testing framework,
    and not document generation tools. 
But that is all easily fixed with a few small scripts (written in, you guessed it, GAWK):

Q2: What does GOLD do?    
A2: GOLD is an object layer that extends GAWK with objects, aggregation, polymorphism, inheritance and abstraction (with iterators)   This code has been used to implement:


- GOLDMINE are some data mining tools written in GOLD.
- GOLDSTAR are a set of data mining fairness assurance operators. 
- GOLDEN is a cross-platform development environment for the above,  based on bash, vim and tmux. 
  Note that you do not need to use GOLDEN to write/ run GOLD code. But it helps.


Q3: What does a session with GOLDEN look like?   
A3: Here:

<p align=center><a href="https://github.com/golden/dev/blob/master/etc/img/screen.png"><img src="https://github.com/golden/dev/blob/master/etc/img/screen.png" width=900></a></p>


Q4: How to write GOLD source code files?    
A4:  Use a `*.md` extension
and store your code in  awk "fence blocks"; e.g.

        ```awk
        code
        ```

Q5: How to to write unit tests for a file `x.md`?    
A5:  Create a file, that includes it, called `xok.md`.

Q6: How to run GOLD?   
A6: You will need five files 
  `g,o,l,d,en` 
and a set of libraries (found in `./src/gold/*`).  To get these files, use the [INSTALL](INSTALL.md) instructions.


  <table>
    <tr>
      <td><tt>g</tt></td>
      <td>The GOLD transpiler.</td>
      <td>
        <ul>
          <li>Converts all src/*.md to *.awk files.</li>
          <li>Recursive expand the <tt>@include</tt> commands with the contents
          of those files.</li>
          <li>Implement key access with dots<br>
          e.g. `i.oid=++GOLD.oid`` becomes `i["oid"]=++GOLD["oid"]`.</li>
          <li>Add executable line <tt>#!/usr/bin/env gawk -f</tt> to top of
          file.</li>
          <li>Tell the file was the top level files (using 
          <tt>BEGIN {TOP=FILENAME}</tt></li>
          <li>Change the file's mode with <tt>chmod +x</tt>.</li>
          <li>Runs the resulting .awk</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td><tt>o</tt></td>
      <td>Overlay</td>
      <td>
          Lists the header file in root README.md. Useful for adding a standard
          hearder to <tt>*.md</tt> files.
          <p>Note that if called withe the optional flag <tt>o -r</tt> flag,
          then <tt>o</tt> recursively on all <tt>./src/*.md</tt> files (to give
          everyone the same look and feel).</p>
      </td>
    </tr>
    <tr>
      <td><tt>l</tt></td>
      <td>Lists</td>
      <td>List the markdown headers in this file. Useful while editing
      <tt>*.md</tt> file to add a table of contents.</td>
    </tr>
    <tr>
      <td><tt>d</tt></td>
      <td>Demos</td>
      <td>Run all the <tt>src/i*ok.md</tt> files</td>
    </tr>
    <tr>
      <td><tt>en</tt></td>
      <td>Environment</td>
      <td>
        Set up for using Gold.
        <ul>
          <li>Ensure there exists the <tt>g,o,l,d,en</tt> files (and they are
          executable).</li>
          <li>Ensure that the <tt>./src/gold/\*</tt> files exist (which stores
          the core GOLD files).</li>
        </ul>If they don't exist, they are initialized.
      </td>
    </tr>
  </table>

Note that, in the above, "ensure exists" means that missing files
  will be downloaded but existing files will not be overwritten. This means
  that if you do any local config, those changes are safe.


