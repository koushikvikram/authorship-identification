# Implementing a Standard Text Mining Workflow

## Objective
We are handed a note from an anonymous source. 
The source states they don’t know who the person is that wrote the defaming blog post, 
but they do know it was one of a specific group of people. 
You want to get started immediately and start tracking down the guilty party. 
You’ll need to show your boss something quickly to show that this authorship analysis approach 
can lead to identifying the author.

With that in mind, load the dataset in quickly, and convert it to a usable format. 
From there, apply a standard text mining pipeline to the data, get a classification report, 
and use that to estimate how effective your model will be.

## Workflow
1. Load the dataset from the raw XML files into a more usable format.
2. Create a sample of the dataset for initial experiments.
3. Run a standard text mining pipeline to predict the author ID from their text, 
classifying only between the ten authors in the sample.

### Detailed workflow
1. Load the dataset from the raw XML files into a more usable format.
    - Download the [dataset](https://u.cs.biu.ac.il/~koppel/BlogCorpus.htm) to a data directory
    - Examine the dataset by unzipping and opening a few XML files (open as text files in your favorite text editor).
    - Write the function to load an XML file from disk. 
    Test that when you read the file “5114.male.25.indUnk.Scorpio.xml,” 
    you get an author ID of 5114 and an age of 25, and that the first post 
    starts with “Slashdot raises lots of.”
    - Load all XML files into one big pandas data frame, and save that to a file.
    - Test that you can load the data in from the saved file and that 
    the posts themselves didn’t get altered from the source.

2. Create a sample of the dataset for initial experiments.
    - Sample the data to select all posts by any author from a given list of author IDs.
    - Extract a sample of posts from authors with IDs 
    **[3574878, 2845196, 3444474, 3445677, 828046, 4284264, 3498812, 4137740, 3662461, 3363271].** 
    These are our candidates for writing the defaming blog post.
    
3. Run a standard text mining pipeline to predict the author ID from their text, 
classifying only between the ten authors in the sample.
    - Extract character n-grams from the documents as the features 
    (as opposed to word vectors) for preprocessing.
    - Perform classification on the resulting dataset.
    - Evaluate your classification using the F-score 
    (also known as the F1 score, F1 metric, or F-beta score, among other names).

## Extra Challenge
Try your methodology on other random samples of authors and different sample sizes. 
How effective is this method with two authors? What about 20 authors? 
What about all-male authors under the age of 25?

## Importance to the project
- We need to get some data into our application, and we need to load it in from a dataset that is okay, 
but not great for parsing. Once we have written and tested our application, 
we can rely on this dataset loading code in the future.
- **Setting ‘char’ in TfidfVectorizer is required.** The default of **word** is excellent 
when you are trying to predict what category a person is talking about, 
but **doesn’t work that well for authorship style**. Using words can give the impression 
of working - after all, **most of the time, bloggers talk about one topic.** 
However, this gives false confidence, as those models tend to **fail 
when the author discusses other topics.**
- Once we have a standard data mining setup running, we can use these results 
as a baseline, and experiment with other settings and sample sizes.

## Notes

All datasets have their quirks, and this one is no different. Here are a few shortcuts:
- The files look like XML, but have a few issues that strict XML parsers do not have. 
For instance, special characters are not escaped. 
You can try to parse the files as XML if you are up for a challenge, 
but it is more straightforward, and gets the same answer if you **read the data line-by-line 
and build your dataset using more standard text processing methods.**
- For opening one of the XML files in this dataset, use 
*open(filename, encoding='Windows-1252', errors='ignore')*. 
We’ll deal with how to work out the encoding in the next milestone, 
and the number of ignored errors is small enough that we can ignore them for now.
- Do not try to parse the dates from the dataset. 
They are in lots of different locales. 
Tools like **dateparser** can *nearly* get you to a solution, but there are other issues as well 
(for instance, some blogs have missing dates). **You will not need the dates for this project.**
- pandas supports multiple input/output formats. 
It doesn’t matter too much which size you choose, but I recommend you look past 
the standard CSV or Excel choices. Instead, look at some of the other formats pandas can use.
- From research into authorship analysis, we know that **n-gram values between 2 and 6 tend to do quite well.** 
You can test outside this range, but test within this range first.
- If you are more familiar with text mining, try more advanced text vectorization 
strategies, keeping in mind the underlying goal of the classification 
is authorship analysis, not content/topic analysis. 
**The defaults in most text mining libraries are for content analysis.**

