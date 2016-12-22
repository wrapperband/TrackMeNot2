#TrackMeNot2
**A security / privacy add-on Firefox edition**

TrackMeNot2 contains a number of enhancements and bug fixes over TrackMeNot. This fork documents the changes made to Firefox release of TrackMeNot 0.9.2.

TrackMeNot initiates searches based on RSS feeds to supply key words. It can be used to generate some interesting "random" searches of the internet or "noise" in searches and web interaction to reduce quality of data stored by profilers.


TrackMeNot contains many hard coded variables. This extended version documents how to run a modified version and the changes you can make to include your own customisation.

Currently Firefox add-ons are signed (by Mozilla), so the aim would be to make all variables customise-able. Ultimately, it could be envisaged to include additional self modifying or self customisation to make an updated version called **Browser Bot**, which can be used to "intelligently search" / learn to oppose a Bot teacher, or research and BookMark relevant pages from a seed keywords file.


### Running a customised version of TrackMeNot  

Javascript is an open text code, run by the browser at the time. Mozila insist that any code run as an extension must be signed to avoid tampering. By using the Developer version of Firefox it is possible to run unsigned applications, or include new modifications.

Back-up your settings folder, under .mozilla in GNU/Linux. Or run the developer edition of Firefox in a Virtual environment, VM or jail.  


#### Install the developer edition of Firefox, to run unsigned applications.  

type about:config

scroll down to xpinstall.signatures.required  

change 'Value' from True to False  


#### TrackMeNot  

In Firefox go to tools / add-ons and search and install TrackMeNot.   


### Running your own custom version  

By making it easier to customise all variables is the first step to increasing the security of TrackMeNot. The initial audit shows too many variables are not customise-able thus making the bots actions more trackable.

Without changing the code a new user should update the default blacklist, the RSS feeds are also important to generate web searches.


#### Up-dateable files  

Find where the Firefox extensions are stored. On Ubuntu 16.04 :  

    .mozilla/firefox/20fzsdo8.dev-edition-default/extensions/


Install the source code for the TrackMeNot. Git clone the TrackMeNot directory.  


note that the files are in the compressed directory  

      trackmenot@mrl.nyu.edu.xpi  


Clone the source code         
      
git clone https://github.com/wrapperband/TrackMeNot.git  

The files can be edited and copied into the running system (compressed.xpi), then the system restarted to activate the changes.  


It is relatively easy to copy in an updated version of trackmenot.js . This file is in the root director so arc viewer can handle it.  

Edit the file in the TrackMeNot, then copy it into the compressed .xpi file.  


#### Editing trackmenot.js  var zeitgeist  

The trackmenot.js is a text file contains a number of variables to tweak, the first is **var zeitgeist** an array of "influential" words (need audit).  


##### timeout

The next variable to adjust is timeout. Any common factor can indicate usage, a range is harder to eliminate. When adjusting less important, or harder to monitor parameters consider that the change from norm could also be trackable. May be worth lengthening. 

var tmn_timeout = 6000;


##### burstTimeout  

A low value parameter but common factor. Increased slightly. 

var burstTimeout = 6000;


#### Editing options.html 

The options.html is the next file which can be adjusted.  


##### zeitgeist  

Note : that it is sometime difficult to update a hierarchy of folders into a archived or compressed file. I had to back up the TrackMeNot archive file, delete the whole data directory, then import the new data directory including the updated option.html file. 

On line 157 of options.html you can see the modified time stamps from the  original edition. Again common patterns, in this case temporal repetition rates are easily identified and eliminated.

 <select id ="trackmenot-opt-timeout">
                                                        <option data-l10n-id="tmn.option.freq.10pm" id="t0" value="6000" > </option>
                                                        <option data-l10n-id="tmn.option.freq.5pm" id="t1" value="12000"  > </option>
                                                        <option data-l10n-id="tmn.option.freq.1pm" id="t2" value="60000"  > </option>
                                                        <option data-l10n-id="tmn.option.freq.30ph" id="t3" value="120000" > </option>
                                                        <option data-l10n-id="tmn.option.freq.10ph" id="t4" value="360000" > </option>
                                                        <option data-l10n-id="tmn.option.freq.1ph" id="t5" value="3600000"> </option>
</select>

Customise the repetition times, in the future the system could also vary these times to make it a less "track-able" feature. In the future simply adding or removing  a small random rmber will make each person and session have different base timings. The noise will make it more difficult to identify.  


#### Adding none search links  

As long as the text "trackmenot" is included in the description the link will be accepted as a search URL. The search terms will be included, which can be used to replace parts of identification codes.  

Ideally the bot should follow links, which is envisaged for the Browser Bot version. 


#### Customising Initiation Options   

trackmenot.js  
function initOptions() {
		enabled = true;
		tmn_timeout = 60034;
		burstMode = true;
		setSearchEngine();
		useTab = false;
		useBlackList = true;
		useDHSList = false;
		kwBlackList = ['satan', 'neo', 'acordians'];
		saveLogs = true;
		disableLogs = false;

Showing some slight modifications to initial values to customise personal versions. These should be automatically, or personally configurable,, the blacklist defaults and function requires further audit and consideration.  


#### Customising number of search terms to use  

trackmenot.js function doSearch()    

doSearch() contains the  algorithm to decide the number of search terms to use.  


Adjust the number of words before random further words are added.
if (queryWords.length > 3) {


Adjust the number of additional parameter range.
var unsatisfiedNumber = roll(1, 4);


#### Customising function queryToURL(url, query) {  


trackmenot.js function queryToURL(url, query)  
if (Math.random() < 0.9)  

A randomising function that should be variable, or adjusted from the default value.  


#### Customising the Tab activated message  

When a Tab search is taking place a message tells it is TrackMeNot's Tab. The update includes a more positive Tab view searches message in file trackmenot.js.

notifications.notify({
                text: "This tab shows TrackMeNot's auto-generated searches",
                
                
#### Customise the burst count  

The burst count in file trackmenot.js  seems excessive at between  3, 10, here it has been customised to between  3, 7 .

		burstCount = roll(3, 7);


#### Varying the repeat search  
		
The function scheduleNextSearch in trackmenot.js handles the timing of the next look-up / search.

	function scheduleNextSearch(delay) {

TrackMeNot is modified with a line from the burst mode to randomise all further searches, in this case between 0.5 and 1.5 of the specified normal value. Over time the searches will average at the specified timing.

		tmn_errTimeout = timer.setTimeout(rescheduleOnError, delay * 3);
		delay += delay * (Math.random() - .5);
		tmn_searchTimer = timer.setTimeout(doSearch, delay);


Note, it is also possible to customise error delays and the burst variability in this function.  


#### function scheduleNextSearch(delay)  

The scheduleNextSearch has been updated to fix some Javascript errors and to start to randomise all search periodicity, whilst keeping to the goal amount of searches per second.
		
The calculation for extending the delay on error has been modified so there is always some delay. [tmn_errTimeout]  

Missing brackets were added to return;  

Zeros were also put before the fractions, e.g. 0.5 to comply with Javascript standards. A return  was added to the  function for the case delay = 0.  
		
		
### TrackMeNot Audit : Comparison of Source Code with released version.  

#### Inconsistencies of Firefox version from Source code  

trackmenot.js 
Source code
Array.prototype.contains = function(obj) {
		var i = this.length;
		while (i--) {
			if (this[i] === obj) {
				return true;
			}
		}
return false;

Firefox release
	function containsObj(arr, obj) {
		var i = arr.length;
		while (i--) {
			if (arr[i] === obj) {
				return true;
			}
		}
		return false;

		
#### Tracking the Trackers  

The help button in the default version points to an external trackable web site. This is adjusted to a  local file, it is possible to download the help through Tor and replace the local file.  


#### Trouble  

Don't let the power get to your head and set click rates too high.  


#### List of alternate RSS feeds  

https://ec.europa.eu/research/index.cfm?pg=rss


#### Anti Bot detection  

Some movement of mouse is required on each page. Scroll down searches. 


#### How and where are custom settings stored in TrackMeNot

The current settings are stored in a json file  
    store.json  

For Ubuntu directory is :  

    .mozilla/firefox/jetpack/simple.storage/
    
 
 
Referances
Ref 1 : SimAttack: private web search under fire : https://jisajournal.springeropen.com/articles/10.1186/s13174-016-0044-x  

    Albin PetitEmail author, Thomas Cerqueus, Antoine Boutet, Sonia Ben Mokhtar, 
    David Coquil, Lionel Brunie and Harald Kosch

Ref 2 : https://www.schneier.com/blog/archives/2006/08/trackmenot_1.html     TracMeNot : Bruce Schneier on Security


