# Wiggins Homework 2: Color Classifiers

**GitHub repository:** [ArthursDent/cpsc1710-labs](https://github.com/ArthursDent/cpsc1710-labs)

This project began as a three-label **Stoplight classifier** that identifies a circle as Red, Yellow, or Green. I then developed a second version that classifies different shades of Red, Green, and Blue using RGB sliders.

## My pages

| Version | Labels | How to interact |
| --- | --- | --- |
| [Stoplight / Color Club](lab-02/color-classifier.html) | Red, Yellow, Green | Train, then use the color picker or preset buttons. |
| [RGB Shade Club](lab-02/rgb-shade-classifier.html) | Red, Green, Blue | Train, then move the RGB sliders or select a preset. Includes **Unknown / no good match**. |

### How to open and operate the pages

1. Download or clone this repository, then open the `lab-02` folder in File Explorer.
2. Double-click `rgb-shade-classifier.html` or `color-classifier.html` to open it in a regular browser.
3. Click **Train the model**.
4. Change the color and watch how the model identifies it. In RGB Shade Club, move the Red, Green, and Blue sliders individually or together.
5. Click **Reset** to start again.

The pages run offline without API keys, paid services, or a build step. Clicking their file links on GitHub displays the source code rather than running the page.

To open a page using a local browser link, first run this command in a terminal inside the `cpsc1710-labs` folder, with Python installed:

```powershell
python -m http.server 8000
```

Then paste one of these addresses into your browser:

- Stoplight: http://localhost:8000/lab-02/color-classifier.html
- RGB shades: http://localhost:8000/lab-02/rgb-shade-classifier.html

These addresses work on your computer while the server is running; they are not public website links. Press **Ctrl+C** in the terminal to stop the server.

## How the classifier makes a prediction

Training calculates an average RGB color for each label. For a new circle, the classifier measures the distance from the chosen RGB value to each average and selects the closest one. It compares colors rather than recognizing objects or traffic lights in photographs.

In RGB Shade Club, each RGB channel is divided by 255 before measuring distance. If the nearest average is more than **0.70** away, the page returns **Unknown / no good match**. This is a fixed demonstration cutoff, not a fourth label learned from examples.

### Two limitations

1. **The available labels are limited.** The original Stoplight version must choose Red, Yellow, or Green, even when the input is purple or pink. The RGB version can reject distant colors, but some unfamiliar colors still receive a known label.
2. **The examples and cutoff affect the result.** Only nine training circles determine the averages. The distance cutoff was chosen for this demonstration, and neither it nor the match scores guarantees that the result agrees with how a person names a color.

## Wiggins Homework 2: One-pixel observations

These are my recorded observations from the [one-pixel demo](lab-02/one-pixel.html).

1. **Clean Split:** The model selected a brightness cutoff of 120, rather than the roughly halfway value of 127 that I expected. The outputs were clean, with no mistakes. My initial guess was that the model chose the lowest viable cutoff; I have not verified that explanation.
2. **One Odd:** I found it interesting that the cutoff did not move. It stayed at 120, and the model made one mistake.
3. **Swapped Labels:** After swapping the labels, the learned cutoff was 0, but there were still six mistakes. This was an interesting result to me.

## My original classification idea

1. My page will classify Red, Yellow, and Green circles.
2. The possible labels are Red, Yellow, and Green.
3. The classifier will look at the RGB color values of different circles.
4. One example for each label is the corresponding light in a traffic light.
5. A visitor should understand how examples help a model distinguish between three color labels, and what happens when a color does not fit those labels.

## Stop 4: Development log

- I asked for a first version with three color labels and tested it against other colors.
- I then changed the idea to various RGB shades and added RGB sliders in a second page.
- Before the Unknown rule was added, setting all three sliders to 0 made the model predict Green. I found this strange because RGB (0, 0, 0) is black.
- I also observed a Blue prediction when Red was at its maximum and Green and Blue were fairly high (240 or more), but below Red.
- I asked Codex about these results. It explained that the model chooses the average training color closest to the input.
- My initial guess involved green being a mixture of yellow and blue. **Clarification:** that is a paint-mixing idea, not how this RGB model works. RGB describes red, green, and blue light. The model compares distances in all three channels; the training averages explain its choices, rather than a rule about mixing paint.
- I added **Unknown / no good match** when an input is too far from every training average. In the updated RGB page, all-zero inputs now return Unknown instead of Green.

## Stop 5: Testing notes

| Case | What I tried | What I observed or learned |
| --- | --- | --- |
| Easy case | Maxed each of the three sliders individually, with the other two at zero. | Tested the pure Red, Green, and Blue inputs. These check the primary colors, rather than every color in the full spectrum. |
| Close case (my original test) | Set all three sliders to zero. | Before the Unknown rule, Green won because its average was closest. Black was still far from every average, so this was not necessarily a close decision between labels. The updated page returns Unknown. |
| Strange case | Tested mixtures, including high values in all three channels, and varied the Green input. | I observed surprising shifts between Red and Blue predictions. The effect depends on all three input values and the learned averages; increasing Green does not always favor one label. I did not record an exact RGB value for every observation. |

I like having the Unknown result as a way to flag a poor match. The three displayed scores still compare only Red, Green, and Blue, even when the page returns Unknown.

## Other experiment

[The 10:30 Classifier](lab-02/sp500-classifier.html) explores an S&P 500 direction prediction using synthetic data. Its results do not establish real-world market performance. See the [project notes](lab-02/sp500-README.md) for details.

## Course materials

- [Lab 1: Meet a deep-learning notebook](lab-01/index.html)
- [Lab 2: From one pixel to your classifier](lab-02/index.html)
- [Lab 2 assignment](lab-02/hw-02-assignment.html)
- [Original one-pixel demo](lab-02/one-pixel.html)

Visit the instructor's [live course hub](https://xiuyechen.github.io/cpsc1710-labs/) to view the original course pages online. That site is separate from the classifier additions in this fork. Assignments are also provided as printable PDFs in their lab folders.

## Credits

Original course materials are by [Xiuye Chen](https://github.com/xiuyechen), developed with Codex, and shared under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

The classifier additions in this fork were created with Codex assistance. Color examples use the [W3C named-color table](https://www.w3.org/TR/css-color-3/#svg-color), grouped into teaching labels for these demonstrations.
