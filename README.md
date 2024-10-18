# Worksheet: Segues and Navigation Controllers in iOS

Understand and practice how to use segues, unwind segues, and navigation controllers in iOS development using Swift and Storyboards.

## Part 1: Setting Up the Project

1. **Create a New Xcode Project:**

   - Open Xcode and create a new "Single View App" project named "TrafficSegues".
   - Set the "Main Interface" field to use `Main.storyboard`.
2. **Initial Layout:**

   - In `Main.storyboard`, drag three `UIViewController` instances onto the canvas.
   - Set the background color of each view as follows:
     - **Red View Controller**: Red background (initial view controller).
     - **Yellow View Controller**: Yellow background.
     - **Green View Controller**: Green background.

## Part 2: Creating Basic Segues

1. **Adding Buttons for Navigation:**

   - On the **Red View Controller**, add a button in the center titled "Go to Yellow".
   - Create another button titled "Go to Green".
2. **Setting Up Segues:**

   - Control-drag from the "Go to Yellow" button to the **Yellow View Controller**, and select "Show" as the segue type. This will create a segue that presents the Yellow View Controller.
   - Repeat the process for the "Go to Green" button, creating a segue to the **Green View Controller**.
3. **Testing the Project:**

   - Run the project to verify that clicking the buttons transitions between the Red, Yellow, and Green View Controllers.

## Part 3: Introducing Navigation Controllers

1. **Embed the Initial View in a Navigation Controller:**

   - Select the **Red View Controller**.
   - Go to the menu and choose `Editor -> Embed In -> Navigation Controller`.
   - Observe how a navigation bar appears at the top of the view.
2. **Navigation Controller Behavior:**

   - Notice that the "Show" segues now appear as push transitions instead of modal presentations.
   - The navigation bar includes a "Back" button automatically when transitioning to Yellow or Green.
3. **Experimenting with the Navigation Bar:**

   - Select the navigation bar in the document outline and inspect the attributes in the Attributes Inspector. You can change the style, color, and appearance.

## Part 4: Configuring Navigation Items

1. **Setting Titles for Each View:**

   - Each `UIViewController` has a `Navigation Item` in the document outline.
   - Set the title for each view in the Attributes Inspector:
     - **Red View Controller**: "Red"
     - **Yellow View Controller**: "Yellow"
     - **Green View Controller**: "Green"
2. **Adding Bar Buttons:**

   - Add a "Dismiss" button to the **Green View Controller**:
     - Drag a `Bar Button Item` onto the right side of the navigation bar.
     - Set the system item to "Done" in the Attributes Inspector.
   - When this button is tapped, it should navigate back to the previous view. This behavior will be implemented shortly.

## Part 5: Unwind Segues

1. **Creating an Unwind Segue:**

   - In the **Red View Controller** class (`ViewController.swift`), add the following method:

     ```swift
     @IBAction func unwindToRed(unwindSegue: UIStoryboardSegue) {
     }
     ```
2. **Connecting the Unwind Segue:**

   - In the storyboard, control-drag from the "Dismiss" button in the **Green View Controller** to the `Exit` icon at the top of the scene.
   - Select the `unwindToRed` action.
3. **Testing the Unwind Segue:**

   - Run the project and navigate to the **Green View Controller**.
   - Tap the "Dismiss" button to return to the **Red View Controller**.

## Part 6: Passing Data Between View Controllers

1. **Adding a Text Field in the Red View Controller:**

   - In the **Red View Controller**, add a `UITextField` above the "Go to Yellow" button.
   - Set the placeholder text for the text field to `"Set text in Yellow"`:

     - In the storyboard, select the `UITextField`.
     - Open the Attributes Inspector and set the "Placeholder" field to `"Set text in Yellow"`.
   - Alternatively, you can set the placeholder programmatically in the `ViewController.swift` file:

     ```swift
     override func viewDidLoad() {
         super.viewDidLoad()
         textField.placeholder = "Set text in Yellow"
     }
     ```
   - Create an `IBOutlet` for the text field in the `ViewController.swift` file:

     ```swift
     @IBOutlet weak var textField: UITextField!
     ```
2. **Adding a Label to the Yellow View Controller:**

   - In the **Yellow View Controller** (Storyboard), drag a `UILabel` into the center of the scene.
3. **Creating and Associating `YellowViewController.swift`:**

   1. **Create the YellowViewController Class:**

      - In Xcode, create a new Swift file:
      - Right-click on the project's file navigator and select `New File from Template -> Cocoa Touch Class`.
      - Name the file `YellowViewController`, and make sure it subclasses `UIViewController`. Also, ensure it's added to the project target.
   2. **Associate the Yellow Scene with `YellowViewController.swift`:**

      - In the storyboard, select the **Yellow View Controller** scene.
      - Open the Identity Inspector (right panel) and set the "Class" field to `YellowViewController` under the "Custom Class" section. This step links the storyboard scene with the `YellowViewController` class.
   3. **Connect the Label to the Code:**

      - Control-drag from the `UILabel` in the storyboard to the `YellowViewController.swift` file to create an `IBOutlet` for the label. The outlet should look like this:

        ```swift
        @IBOutlet weak var displayLabel: UILabel!
        ```
   4. **Set Up the Class Definition:**

      - In `YellowViewController.swift`, update the class definition to include an optional property for the text to be displayed:

        ```swift
        import UIKit

        class YellowViewController: UIViewController {
            @IBOutlet weak var displayLabel: UILabel!
            var textToDisplay: String?

            override func viewDidLoad() {
                super.viewDidLoad()
                displayLabel.text = textToDisplay
            }
        }
        ```
4. **Setting Up the Data Passing with Storyboard Segue:**

   - In `ViewController.swift` (Red View Controller), override the `prepare(for:sender:)` method to pass the text from the text field to `YellowViewController`:

     ```swift
     override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
         if let destinationVC = segue.destination as? YellowViewController {
             destinationVC.textToDisplay = textField.text
         }
     }
     ```
5. **Testing the Data Passing:**

   - Run the Project:
     - Enter some text in the **Red View Controller**'s text field and tap the "Go to Yellow" button.
     - The text should now appear in the `UILabel` in the **Yellow View Controller**.

## Part 7: Programmatic Segue Execution

1. **Adding New UI Elements:**

   - In the **Red View Controller**, add a new `UISwitch` and position it below the existing text field.
   - Add a new button titled "Go to Green?" below the switch.
2. **Creating New Segues:**

   - Keep the existing segues for "Go to Yellow" and "Go to Green" buttons.
   - Add two new segues:
     - Control-drag from the **Red View Controller** to the **Green View Controller** and set the segue identifier to "GreenProgrammatic".
     - Control-drag from the **Red View Controller** to the **Yellow View Controller** and set the segue identifier to "YellowProgrammatic".
3. **Configuring the Segue Logic:**

   - In the `ViewController.swift` file, create an `IBOutlet` for the newly added switch:

     ```swift
     @IBOutlet weak var programmaticSwitch: UISwitch!
     ```
   - Also, create an `IBAction` for the "Go to Green?" button:

     ```swift
     @IBAction func goToGreenButtonTapped(_ sender: UIButton) {
         if programmaticSwitch.isOn {
             performSegue(withIdentifier: "GreenProgrammatic", sender: nil)
         } else {
             performSegue(withIdentifier: "YellowProgrammatic", sender: nil)
         }
     }
     ```
4. **Testing the New Programmatic Segue:**

   - Run the project in the simulator.
   - Toggle the new switch and click the "Go to Green?" button:
     - If the switch is **on**, the app will segue to the **Green View Controller**.
     - If the switch is **off**, the app will segue to the **Yellow View Controller**.

---

## To Learn More

For additional learning resources and deeper insights into working with segues, navigation controllers, and data sharing in iOS development, explore the following materials:

1. **Developing in Swift - Fundamentals - Unit 3, Lesson 7 - "Segues and Navigation Controllers"**
2. **Developing in Swift - Fundamentals - Unit 3, "Guided Project: Personality Quiz"**
