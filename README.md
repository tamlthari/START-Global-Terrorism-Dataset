# START Global Terrorism Dataset
We analyze the START Global Terrorism Dataset using Plotly, ANOVA test volcano plots, Topological Data Analysis (TDA) and Google Data Studio.
Our notebook is published at [Google Colab](https://colab.research.google.com/drive/1QA3N-P9Hsrtvio0vec-jE7yOUgsKCZtG).
Our Google Data Studio is at [link](https://datastudio.google.com/s/uerw2If2fNI).
Our team includes: Ly Tran ([github](https://github.com/TranLySFW)) and Tuc Nguyen ([github](https://github.com/ngtrituc))

## Python Dependencies
1. [GDSCTools](https://gdsctools.readthedocs.io/en/master/installation.html) and dependencies
2. Bokeh
3. Scikit-tda [Kepler-Mapper](https://github.com/scikit-tda/kepler-mapper) and dependencies
4. Plotly

## ANOVA Volcano plots
1. Preprocessing data
    - We use `pandas.get_dummies` to "one-hot encode" categorical data
    - Our features include **Attack Type, Target Type, Victim Nationality, Group Name, Weapon Type, Province State, Region**
    - Our labels include **Number Killed, Number Killed US, Success, Damaged Property Value, Number Wounded, Number Wounded US, Number Hostages kidnapped, Number Hostages Kidnapped US, Number of Days kidnapped, Number of Hostages Released, Ransom Amount, Ransom Amount US sources, Ransom Paid, Ransom Paid from US sources**
    - We split our labels and features data into regions for faster ANOVA test runs with `gdsctools` standalone application.
2. Visualizing ANOVA test results with volcano plots
    - For understanding volcano plots see this [link](https://discover.nci.nih.gov/microarrayAnalysis/Statistical.Tests.jsp)
    - we use `bokeh` to plot for each regions
    - standalone volcano plots published at this [link](https://tamlthari.github.io/START-global-terrorism/anova-volcano/index.html)

## Topological Data Analysis
1. About the data
    - The columns used to build the graph are `['nkill','nkillus','nkillter','nwound','nwoundus','nwoundte','propvalue','nhostkid','nhostkidus','ndays','ransomamt','ransomamtus','ransompaid','ransompaidus','nreleased']`
    - We set `NaN` values to zero
    - We have two different graph setups
    - **Graph 1:** 
        - We combine two lens: 1-D lens with `sklearn ensemble IsolationForest` and 1-D lens with `kmapper.KeplerMapper` L2norm projection   
        - We use `sklearn.cluster.KMeans` as clusterer in our graph
    - **Graph 2:**
        - We use `kmapper` projection with `sklearn.manifold.TSNE`
        - We use `sklearn.cluster.DBSCAN` as clusterer in our graph
    - Set tooltips and color_function as number killed and weapon type
    - Links to example graphs: 
        -   Link to graph l2norm kmeans tooltip weapontype https://tamlthari.github.io/START-global-terrorism/kepler-weapontype1-l2normkmeans/tooltip-weapontype/index.html#

        -   Link to graph l2norm kmeans tooltip number killed https://tamlthari.github.io/START-global-terrorism/kepler-weapontype1-l2normkmeans/tooltip-nkill/index.html#

        -   Link to graph dbscan tsne tooltip weapontype https://tamlthari.github.io/START-global-terrorism/kepler-weapontype1-tsnedbscan/tooltipweapontype/index.html#

        -   Link to graph tsne dbscan tooltip number killed https://tamlthari.github.io/START-global-terrorism/kepler-weapontype1-tsnedbscan/tooltipnkill/index.html#

2. Pearson correlation of number killed and firearms fraction
    - For each node we calculate the average number killed for all node members and fraction of firearms weapon type for all node members.
    - We use `scipy.stats.pearsonr` to calculate the Pearson correlation


## Plotly
1. Geomap: 
    -   We use `plotly.graph_objects` to create the global map of terrorist events. We enable hover text to show the event details including number killed, by group, motive, date.
2.  Successful attacks by regions:
    -   We use `plotly.graph_objects` to create the interactive plot showing yearly event counts by region
3.  Number of success/fail attacks in North Africa & Middle East/South Asia/North America/Southeast Asia:
    -   We use `plotly.graph_objects` to create stacked bar charts showing number of success/fail events for the years 1970-2017.
4.  Stacked bar charts of weapon type number of success attacks by year:
    -   We use `plotly.graph_objects` to create stacked bar charts of weapon type number of success attacks for the years 1970-2017
