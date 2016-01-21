TrelloBrau
=======

A simple server to combine the awesomeness of Trello with automated data collection
during the fermentation of beer.

#Todo:
* Watch Trello board for cards being moved into the 'Fermentation' column
* When a card is added to 'Fermentation':
    * Add a comment to the card containing the current date/time as
        'fermentation start date'
    * Add another comment with a link to the plot of fermentation temperature
        starting at "fermentation start date"
    * Look through all comments of the card and look for the recipe comment
        (denoted by `Recipe:`) as the first bit of text. Parse the fermentation
        temperature from the recipe comment, and set the temperature on the
        particle photon.
* When a card is removed from Fermentaion column:
    * Add a comment to denote the 'fermentation end date'
    * Generate a plot of temperature data from start to end of Fermentaion and
        attach it to card.
    * Attach a CSV containing temperature data for the fermentation period as
        well.
* Ideally, the server will be stateless: on startup, and on any change event,
  it can look at every card in the *fermentation* and *bottled* columns, and
  perform the above steps as necessary. This will allow a user to edit the start
  and end date for fermentation, and the plots and data will update
  automatically.

#Functions:
* parseCard: will take an entire card as input, and will parse it into a
    brew-state object containing all necessary details:
    * fermentationTemp (parsed from Recipe)
    * fermentationStart (comment)
    * fermentationEnd (comment)
    * fermentationPlotLink (comment)
    * fermentationPlotAttachment
    * fermentationDataAttachment
    * column (pre-ferment, ferment, post-ferment)

* startFermentation: will take a card object as input, and add the fermentation
    start time as a comment;

* endFermentation: will take a card object as input add add the fermentation end
    time as a comment;

* getFermentationPlot: will take a start-time and an end-time, and Generate
    the fermentation plot (probably with M2X API), or URL to the plot.

* linkFermentationPlot: will add a comment with a link to an auto-updating plot
    that starts at fermentationStart and ends `now`.

* putFermentationPlot: will take a card object as input and attach the
    fermentation plot or update it

* getFermentationData: Same as above, but will generate a text file of data

* putFermentationData: will take a card object as input and attach the
    fermentation data or update it

* setFermentationTemp: will set fermentation temp using particle API. Should
    only be called by startFermentation.
