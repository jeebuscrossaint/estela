[Zhongzhou/ESTELA-physics-problem-bank: Isomorphic physics problem banks created with the assistance of GenAI](https://github.com/Zhongzhou/ESTELA-physics-problem-bank)

The test preparation system consists of two components:
1. Problem visualizer and picker
2. test builder (yaml to latex converter).

### Problem Visualizer:

The problem builder will ideally allow an instructor to do the following:

* Select a course and a topic in the course.

* Under the topic, view a thumbnail or a quick preview of one problem from each of the problem banks under the topic folder.

* Either select a problem bank or expand the problem bank to view all the problems inside the bank. 

* Note that the challenge here is how to properly display Latex math equation on screen. Maybe the problems need to be converted into css first? Or take the yaml-markdown -> pdf pathway?

* Also note that some problems contain figures, which are in separate 'figure' folder.
* Hint: maybe to speed up loading you can pre-generate a thumbnail of the first question in each bank and store the thumbnails somewhere.
### The test builder

* After the user selected one or more problem banks, the user can start to build multiple exam versions using the utility.
* I'm imagining a "shopping cart" type of interface in which the user can see how many problem banks have been selected, and which topics are covered. for example, 2 problem banks in forces, 1 problem bank in newton's Laws of motion, etc.
* the user can then select "preview exam" and see a generated pdf of one version of the exam. (the first step might be to simply display the latex instead of rendering it? or would it be easier to preview in html/CSS?)
* the user can then select to generate n number of versions of the paper exam. n being  1-10 for example.
* Finally the user hits generate, and the utility generates the following:
	* the user specified version of exams, with each exam containing different isomorphic problems from the selected problem bank.
	* one answer sheet for each version.
	The user can then download latex (and maybe pdf as well?) to edit it however they want.

Clearly it is impossible to build all the features from the beginning. 


There are two options for the end product and you can let me know which option you think is the most suitable as a first step:

1. Hosting it on a free web server such as PythonAnywhere or Render or Vercel. Streamlit Community Cloud can also be an option
2. Creating an installable program using pyinstaller and/or inno setup (or just ask AI about those options)
3. We might also try streamlit app and host it on 