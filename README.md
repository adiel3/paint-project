# Data and analysis for the paint-or-poetry website

 I created the website ["Paint or poetry?"](https://adiel3.github.io/paint-or-poetry/) as my third and final project for Lede 2024. This is the data analysis repository for the project. The website repository can be found [here.](https://github.com/adiel3/paint-or-poetry)
 
## The data extraction
I wanted to download a dataset of all the paint colors on Benjamin Moore's website and their corresponding details. I was most interested in the color names and descriptions, but wanted as much additional information as possible. Getting the data proved to be the largest task of this project and an excellent learning experience for extracting data from websites.

My first attempt to get this data was through the paint search tool's undocumented API, which pulled more than a dozen data points about each paint, including the name, description, color hex code, and primary color family. 

There was no one master list of all the paint colors that I could find, they were split into nine color family pages, with up to 796 colors in each family. The color family pages only had the a few pieces of data about each color, and did not include the descriptions, so the API seemed like the faster method for getting color details. 

I initially tried running the name of each color family through the API, but got back far fewer results than the "over 3,500 paint colors" the company advertised. 

So then I turned to web scraping to get the full list of color names. The color family pages included pagination, showing 49 results per page. To get all the colors, I needed to click through a large number of pages, so I created a web scraper using playwright to automate that clicking, than ran it for each color family.

I then added a color family label to the results I got and combined the colors into one dataframe, checking for repeats. 

I took that list of color names back to the undocumented API, writing new code to search each color name and retrieve only the first result, and build a dataframe of those results. There were over 2,000 additional colors to search, so I first tested it on a small sample, then ran the full code.

In the end, I was able to build a dataframe of 3,657 paint colors, their descriptions and other details.

## The classification and analysis
This dataset of paint color descriptions provided an opportunity to learn how to interact with a large language model via python and get ChatGPT to classify the descriptions several different ways.

Using the Instructor library, I asked the model several classification questions about the paint color descriptions, first with yes/no questions, then with more categories. I was unable to feed the model training data, due to a lack of time, but would have liked to have tried that, as one category of classification was unsuccessful, but likely could have worked with some training data and tweaking of the prompt.

I then grouped the model's classification by color family to see if there were any trends. I also took the smaller subset of descriptions classified by the model as something other than a standard paint description, and manually classified them for comparison. 

While my initial hypothesis was that there would be a number of descriptions that read like poems, classification revealed there were far more that resembled lines from a story. I took the list of paint color descriptions from the "story" category and asked ChatGPT to reorder them into a cohesive story. This was done in the web interface for the model, and so is not in the repository.

I then plugged the resulting article text back into my python notebook and matched the color hex code to each sentence, then used a for loop to create html code so that each line of the story would appear in the color it described.
 
## The presentation
 The website was built from the same basic template from Jonathan Soma as Project 1, but used far more html style, particularly for the story at the end of the article. With so many different colors, accessibility and contrast were difficult for that section. Multiple variations of highlighting, borders and backgrounds were attempted. None were perfect, and in the end, the one that got the effect across most and looked cleanest was simply matching the text color hex to its description.

 Datawrapper was used for two graphics and photos were generously provided by Sara Miller.

### A note
There are multiple noetbooks in this repository:

- `API-paint-descriptions.ipynb` is the notebook for the undocumented API calls
- `API-data-to-df.ipynb` in the data/created subfolder turned extracted csvs into one dataframe for use in other notebooks
- `descriptions-analysis.ipynb` is where most of the calculations, cleaning, and checks for missing data occurred. 
- `descriptions-classifying.ipynb` is the notebook for the interactions with the large language model via Instructor
- `scraping-paint-families.ipynb` is the notebook for scraping paint names via Playwright

