## licensekit

licensekit generates images containing license attributions for use in video games to comply with license attribution requirements. License attributions are shown as static images like this in many major games (such as Final Fantasy XV), usually as an option from the title screen.

If your game uses libraries, fonts, audio, or other resources that require you to distribute their licenses with your game, you can use this tool to generate images to be shown from the title screen or some other place in your game. The idea is that showing these pre-generated images is simpler to implement in your game than displaying, formatting, and paginating a giant wall of text.

If you have questions either open an issue or contact me on Twitter: [@_mrtenda](https://twitter.com/_mrtenda)

Made with [html2canvas](https://github.com/niklasvh/html2canvas/) and jQuery.

### Example Output Images

![Example Page 1](https://user-images.githubusercontent.com/1281326/71454049-aecd1c00-2786-11ea-8a65-6a3d29842e40.png)
![Example Page 2](https://user-images.githubusercontent.com/1281326/71454052-affe4900-2786-11ea-821f-27bf91d86905.png)

### Instructions

1. Download or [git clone](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository) the repository to your computer.
2. Open a terminal and `cd` to the directory where the code is.
3. Generate the website using Jekyll. The most foolproof way to do this would be to install Docker, then run the command below:
    - On OS X: 
    
      ```docker run --rm --label=jekyll --volume=`pwd`:/srv/jekyll -it jekyll/jekyll jekyll b```
    - On Windows:
    
      ```docker run --rm --label=jekyll --volume=%CD%:/srv/jekyll -it jekyll/jekyll jekyll b```
      
    (If you'd prefer not to use Docker, you can install Jekyll as per the [official instructions](https://jekyllrb.com/docs/) and then just run `jekyll b`)
4. A `_site` directory should have been created. Open `_site/index.html` in your web browser.
5. Press the "Create Images" button on the page. Images should appear at the bottom of the page with example data. These are the images you can save from your browser and then use in your game.

Now that you've gotten everything running with example data, the next step is to put in the actual data you want.
1. Add each license as a `.txt` file in the `_includes/licenses` directory. You can delete the example ones in there.
2. Open `_data/licenses.yml` in a text editor and follow the format in there to point to your license files.
3. Open `_data/sounds.yml` in a text editor and add any sound effects you'd like to attribute.
   - If you don't want to include any attributions for sound effects, just delete the contents of this file.
4. Repeat step 3-5 in the first list above to re-generate the website and images.
5. Adjust the parameters at the top of the page as needed. Be sure that the "Pages" value is high enough to show all of your licenses.

### Customization

You can customize the visuals and text as much as you like by editing the following files:
- `index.md`
- `static/css/style.css`

### Disclaimer

The creator of this software is neither a lawyer nor a law firm, and the contributors to this service are not acting as your attorney. The authors cannot be responsible for the consequences of use.
