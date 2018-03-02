# D365FO-oData-T4-MultiFile-Templates
Allow breaking oData client generated files into multiple files so you do not end up with one huge 50+MB file that crashes Visual Studio when debugging

NOTE: I used Visual Studio 2015 in my testing.

To find the edits to the standard oData T4 template, you can search for "xxce" which is the prefix used for any customizations. 

To use these files:
  1. Install the "OData v4 Client Code Generator" Visual Studio extension (Open Visual Studio > Tools > Extensions and Updates > search for "OData v4 Client Code Generator" published by Microsoft)
  2. In your project, add new item, and select "OData Client", name the file "D365ODataClient.tt"
  3. Replace, the generated tt and ttinclude files with the file from this project

To refresh the data entity client
  1. Open your browser of preference. 
  2. Navigate to your D365 instance base URL to get auth token first.  For ex: https://usnconeboxax1aos.cloud.onebox.dynamics.com
  3. After logged in, go here: {baseURL}/data/$metadata For ex: https://usnconeboxax1aos.cloud.onebox.dynamics.com/data/$metadata
  4. Allow it to load, might take several minutes. 
  5. After it finishes loading, then right click and download the file contents to c:\temp\data$metadata.xml
  6. Open the D365ODataClient.tt file
  7. Edit this line to point to the file:
		public const string MetadataDocumentUri = "file:///C:/Temp/data$metadata.xml";
  8. Save the .tt file, and click "OK" when prompted to refresh it.
    If you aren't prompted when saving the file, then right click on the file and choose "run custom tool"
  9. This will regenerate the odata client classes located under the D365ODataClient namespace (or whatever you have set as the NamespacePrefix in the D365ODataClient.tt file), so that you can use any new Data Entities, or access newly added fields/actions/etc.


Credits: 
I leveraged this awesome helper class written by Damien Guard (with a few tweaks) to break out the files.  
https://github.com/damieng/DamienGKit/blob/master/T4/MultipleOutputHelper/MultipleOutputHelper.ttinclude

More info about this MultipleOutputHelper here: https://damieng.com/blog/2009/11/06/multiple-outputs-from-t4-made-easy-revisited 
