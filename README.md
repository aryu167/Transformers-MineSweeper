// Name: ARYAN SHAH
// Date: May 31
// Purpose: TO CODE A WORKING MINESWEEPER
import java.awt.*;
import java.awt.event.*;
import javax.swing.*;
import java.applet.Applet;
import java.io.*;
import java.applet.*;

public class GridStarter extends Applet implements ActionListener
{
    // Panels for the different screens of the game
    Panel p_card;
    Panel card1, card2, card3, card4, card5, card6;
    CardLayout cdLayout = new CardLayout ();
    boolean difficultySelected = false;

    // Flag button and status
    JButton flagPic;
    boolean flagOn = false;

    // Game grid and related arrays to track the state of the game
    int row = 18;
    int col = 15;
    int b[] [] = new int [row] [col];      // Mines and numbers
    int show[] [] = new int [row] [col];   // Revealed cells
    int flags[] [] = new int [row] [col];  // Flagged cells
    int reset[] [] = new int [row] [col];  // Reset grid
    int f = 0;                             // Number of flags
    //Sound file for game.
    AudioClip soundFile;
    JLabel flag;
    JButton pics[] = new JButton [row * col];
    int sqDimension = 23;
    String picStart = "c";
    String picFileType = ".png";
    JLabel turnpic, level;
    // All the colours,fonts and size for my buttons, titles and grid
    Color backgroundColour = (new Color (36, 21, 14));
    Color buttonColour = (new Color (210, 29, 42));
    Color buttonText = Color.white;
    Color titleColour = (new Color (210, 29, 42));
    Font titleFont = new Font ("Arial", Font.PLAIN, 30);
    Font promptFont = new Font ("Arial", Font.PLAIN, 20);
    Dimension boardSquare = new Dimension (20, 20);

    // Initialize the applet and setup the initial screen
    public void init ()
    {
        soundFile = getAudioClip (getDocumentBase (), "mine.snd");
        soundFile.loop ();
        p_card = new Panel ();
        p_card.setLayout (cdLayout);

        // Setup different screens
        opening ();
        instructions ();
        settings ();
        gameScreen ();
        winScreen ();
        loseScreen ();

        // Set the size of the applet and add the main panel
        resize (450, 600);
        setLayout (new BorderLayout ());
        add ("Center", p_card);
    }


    // Opening screen setup
    public void opening ()
    {
        card1 = new Panel ();
        card1.setBackground (backgroundColour);

        // Add button for the next screen
        JButton next = new JButton (createImageIcon ("intro.png"));
        next.setPreferredSize (new Dimension (450, 600));
        next.setActionCommand ("s2");
        next.addActionListener (this);
        next.setBorder (null);
        next.setBorderPainted (false);

        card1.add (next);
        p_card.add ("1", card1);
    }


    public void instructions ()
    {
        // Setup the instructions screen
        card2 = new Panel ();
        card2.setBackground (backgroundColour);

        // Button to go to the game screen
        JButton gameScreen = new JButton (createImageIcon ("rules.png"));
        gameScreen.setActionCommand ("s3");
        gameScreen.addActionListener (this);
        gameScreen.setBorder (null);
        gameScreen.setBorderPainted (false);

        card2.add (gameScreen);
        p_card.add ("2", card2);
    }


    public void settings ()
    {
        // Setup the settings screen
        card3 = new Panel ();
        card3.setBackground (backgroundColour);

        // Title label for the settings screen
        JLabel title = new JLabel ("CHOOSE YOUR DIFFICULTY:");
        title.setFont (titleFont);
        title.setForeground (titleColour);

        // Difficulty buttons
        JButton ez = new JButton ("Ultra Easy(5)");
        ez.setForeground (buttonText);
        ez.setActionCommand ("ez");
        ez.addActionListener (this);
        ez.setFont (promptFont);
        ez.setPreferredSize (new Dimension (250, 50));
        ez.setBorder (BorderFactory.createBevelBorder (0, new Color (35, 115, 219), new Color (35, 115, 219)));
        ez.setBackground (buttonColour);

        JButton easy = new JButton ("Easy(20)");
        easy.setForeground (buttonText);
        easy.setFont (promptFont);
        easy.setActionCommand ("easy");
        easy.addActionListener (this);
        easy.setPreferredSize (new Dimension (250, 50));
        easy.setBorder (BorderFactory.createBevelBorder (0, new Color (35, 115, 219), new Color (35, 115, 219)));
        easy.setBackground (buttonColour);

        JButton medium = new JButton ("Medium(35)");
        medium.setForeground (buttonText);
        medium.setActionCommand ("medium");
        medium.addActionListener (this);
        medium.setFont (promptFont);
        medium.setPreferredSize (new Dimension (250, 50));
        medium.setBorder (BorderFactory.createBevelBorder (0, new Color (35, 115, 219), new Color (35, 115, 219)));
        medium.setBackground (buttonColour);

        JButton hard = new JButton ("Hard(50)");
        hard.setForeground (buttonText);
        hard.addActionListener (this);
        hard.setActionCommand ("hard");
        hard.setFont (promptFont);
        hard.setPreferredSize (new Dimension (250, 50));
        hard.setBorder (BorderFactory.createBevelBorder (0, new Color (35, 115, 219), new Color (35, 115, 219)));
        hard.setBackground (buttonColour);

        JButton vhard = new JButton ("Ultra Hard(65)");
        vhard.setForeground (buttonText);
        vhard.setActionCommand ("vhard");
        vhard.addActionListener (this);
        vhard.setFont (promptFont);
        vhard.setPreferredSize (new Dimension (250, 50));
        vhard.setBorder (BorderFactory.createBevelBorder (0, new Color (35, 115, 219), new Color (35, 115, 219)));
        vhard.setBackground (buttonColour);

        // Entrance button to go to the game screen
        JButton entrance = new JButton ("To the game");
        entrance.setActionCommand ("s4");
        entrance.addActionListener (this);
        entrance.setForeground (buttonText);
        entrance.setFont (promptFont);
        entrance.setPreferredSize (new Dimension (250, 50));
        entrance.setBorder (BorderFactory.createBevelBorder (0, new Color (35, 115, 219), new Color (35, 115, 219)));
        entrance.setBackground (buttonColour);

        // Label to show selected difficulty level
        level = new JLabel ("       Difficulty:         ");
        level.setPreferredSize (new Dimension (250, 50));
        level.setForeground (titleColour);
        level.setFont (promptFont);

        // Panels for organizing components
        Panel p = new Panel ();
        Panel p2 = new Panel ();
        Panel p3 = new Panel ();
        Panel p4 = new Panel ();
        Panel p5 = new Panel ();
        Panel p6 = new Panel ();
        Panel p7 = new Panel ();

        // Adding components to individual panels
        p.add (easy);
        p2.add (medium);
        p3.add (hard);
        p4.add (entrance);
        p5.add (level);
        p6.add (ez);
        p7.add (vhard);

        // Adding components to the main settings panel
        card3.add (title);
        card3.add (p6);
        card3.add (p);
        card3.add (p2);
        card3.add (p3);
        card3.add (p7);
        card3.add (p5);
        card3.add (p4);

        // Adding the settings panel to the card layout
        p_card.add ("3", card3);
    }


    public void gameScreen ()
    {
        // Setup the game screen
        card4 = new Panel ();
        card4.setBackground (backgroundColour);

        // Title label for the game screen
        JLabel title = new JLabel ("M I N E S W E E P E R");
        title.setFont (titleFont);
        title.setForeground (titleColour);

        // Panel for flag status
        Panel p2 = new Panel ();
        flagPic = new JButton (createImageIcon ("cover.png"));
        flagPic.addActionListener (this);
        flagPic.setActionCommand ("flag");
        JLabel ins = new JLabel ("Flag Status?");
        ins.setForeground (titleColour);
        flag = new JLabel ("     Number of Flags: " + f);
        flag.setForeground (titleColour);
        p2.add (ins);
        p2.add (flagPic);
        p2.add (flag);

        // Grid panel for the game board
        Panel grid = new Panel (new GridLayout (row, col));
        int m = 0;
        for (int i = 0 ; i < row ; i++)
        {
            for (int j = 0 ; j < col ; j++)
            {
                pics [m] = new JButton (createImageIcon (picStart + b [i] [j] + picFileType));
                pics [m].setPreferredSize (new Dimension (sqDimension, sqDimension));
                pics [m].setActionCommand (m + "");
                pics [m].addActionListener (this);
                pics [m].setBorder (null);
                grid.add (pics [m]);
                m++;
            }
        }

        // Panel for additional buttons
        Panel p3 = new Panel ();
        Panel p4 = new Panel ();

        // Reset button
        JButton reset = new JButton ("Again");
        reset.addActionListener (this);
        reset.setActionCommand ("reset");
        reset.setBorder (BorderFactory.createBevelBorder (0, new Color (35, 115, 219), new Color (35, 115, 219)));
        reset.setPreferredSize (new Dimension (100, 30));
        reset.setBackground (buttonColour);
        reset.setForeground (buttonText);
        p3.add (reset);

        // Instructions button
        JButton instruct = new JButton ("Instructions");
        instruct.addActionListener (this);
        instruct.setActionCommand ("instruct");
        instruct.setBorder (BorderFactory.createBevelBorder (0, new Color (35, 115, 219), new Color (35, 115, 219)));
        instruct.setPreferredSize (new Dimension (120, 30));
        instruct.setBackground (buttonColour);
        instruct.setForeground (buttonText);
        p3.add (instruct);

        // Settings button
        JButton settings = new JButton ("Settings");
        settings.addActionListener (this);
        settings.setActionCommand ("settings");
        settings.setBorder (BorderFactory.createBevelBorder (0, new Color (35, 115, 219), new Color (35, 115, 219)));
        settings.setPreferredSize (new Dimension (100, 30));
        settings.setBackground (buttonColour);
        settings.setForeground (buttonText);
        p3.add (settings);

        // Save button
        JButton SAVE = new JButton ("Save");
        SAVE.addActionListener (this);
        SAVE.setActionCommand ("save");
        SAVE.setBorder (BorderFactory.createBevelBorder (0, new Color (35, 115, 219), new Color (35, 115, 219)));
        SAVE.setPreferredSize (new Dimension (100, 30));
        SAVE.setBackground (buttonColour);
        SAVE.setForeground (buttonText);
        p4.add (SAVE);

        // Load saved game button
        JButton savedgame = new JButton ("Saved Game");
        savedgame.addActionListener (this);
        savedgame.setActionCommand ("savegame");
        savedgame.setBorder (BorderFactory.createBevelBorder (0, new Color (35, 115, 219), new Color (35, 115, 219)));
        savedgame.setPreferredSize (new Dimension (100, 30));
        savedgame.setBackground (buttonColour);
        savedgame.setForeground (buttonText);
        p4.add (savedgame);

        // Adding components to the main game screen panel
        card4.add (title);
        card4.add (p2);
        card4.add (grid);
        card4.add (p3);
        card4.add (p4);

        // Adding the game screen panel to the card layout
        p_card.add ("4", card4);
    }


    public void redraw ()
    {
        // Update the icons of the game board based on the current state
        int m = 0;
        for (int i = 0 ; i < row ; i++)
        {
            for (int j = 0 ; j < col ; j++)
            {
                if (show [i] [j] == 0 && flags [i] [j] == 0)
                    pics [m].setIcon (createImageIcon ("cover.png"));
                else if (show [i] [j] == 1 && flags [i] [j] == 0)
                    pics [m].setIcon (createImageIcon (b [i] [j] + ".png"));
                else if (show [i] [j] == 1 && flags [i] [j] == 1)
                    pics [m].setIcon (createImageIcon ("flag.png"));
                m++;
            }
        }
    }


    public void reveal ()
    {
        // Reveal all mines on the board
        int m = 0;
        for (int i = 0 ; i < row ; i++)
        {
            for (int j = 0 ; j < col ; j++)
            {
                pics [m].setIcon (createImageIcon (b [i] [j] + ".png"));
                m++;
            }
        }
    }


    public void addMines (int amt)
    {
        // Randomly place mines on the board
        for (int i = 0 ; i < amt ; i++)
        {
            int x = (int) (Math.random () * row);
            int y = (int) (Math.random () * col);
            while (b [x] [y] != 0)
            {
                x = (int) (Math.random () * row);
                y = (int) (Math.random () * col);
            }
            b [x] [y] = 10;
        }
    }


    public void neighbours ()
    {
        // Calculate the number of neighbouring mines for each cell
        for (int i = 0 ; i < row ; i++)
        {
            for (int j = 0 ; j < col ; j++)
            {
                if (b [i] [j] != 10)
                {
                    int count = 0;
                    if (i - 1 >= 0 && j - 1 >= 0 && b [i - 1] [j - 1] == 10)
                        count++;
                    if (i - 1 >= 0 && b [i - 1] [j] == 10)
                        count++;
                    if (i - 1 >= 0 && j + 1 < col && b [i - 1] [j + 1] == 10)
                        count++;
                    if (i + 1 < row && j - 1 >= 0 && b [i + 1] [j - 1] == 10)
                        count++;
                    if (i + 1 < row && b [i + 1] [j] == 10)
                        count++;
                    if (i + 1 < row && j + 1 < col && b [i + 1] [j + 1] == 10)
                        count++;
                    if (j + 1 < col && b [i] [j + 1] == 10)
                        count++;
                    if (j - 1 >= 0 && b [i] [j - 1] == 10)
                        count++;
                    b [i] [j] = count;
                }
            }
        }
    }


    public void open (int i, int j)
    {
        // Recursively reveal empty cells and their neighbors
        if (b [i] [j] != 0)
            return;

        if (i - 1 >= 0 && j - 1 >= 0 && show [i - 1] [j - 1] == 0)
        {
            show [i - 1] [j - 1] = 1;
            if (b [i - 1] [j - 1] == 0)
                open (i - 1, j - 1);
        }

        if (i - 1 >= 0 && show [i - 1] [j] == 0)
        {
            show [i - 1] [j] = 1;
            if (b [i - 1] [j] == 0)
                open (i - 1, j);
        }

        if (j - 1 >= 0 && show [i] [j - 1] == 0)
        {
            show [i] [j - 1] = 1;
            if (b [i] [j - 1] == 0)
                open (i, j - 1);
        }

        if (j - 1 >= 0 && i + 1 < row && show [i + 1] [j - 1] == 0)
        {
            show [i + 1] [j - 1] = 1;
            if (b [i + 1] [j - 1] == 0)
                open (i + 1, j - 1);
        }

        if (i - 1 >= 0 && j + 1 < col && show [i - 1] [j + 1] == 0)
        {
            show [i - 1] [j + 1] = 1;
            if (b [i - 1] [j + 1] == 0)
                open (i - 1, j + 1);
        }

        if (j + 1 < col && show [i] [j + 1] == 0)
        {
            show [i] [j + 1] = 1;
            if (b [i] [j + 1] == 0)
                open (i, j + 1);
        }

        if (j + 1 < col && i + 1 < row && show [i + 1] [j + 1] == 0)
        {
            show [i + 1] [j + 1] = 1;
            if (b [i + 1] [j + 1] == 0)
                open (i + 1, j + 1);
        }
    }


    public boolean win ()
    {
        // Check if the player has won the game by verifying all mines are flagged
        for (int i = 0 ; i < row ; i++)
        {
            for (int j = 0 ; j < col ; j++)
            {
                if (b [i] [j] == 10 && flags [i] [j] != 1)
                    return false;
            }
        }
        return true;
    }


    public boolean lose ()
    {
        // Check if the player has lost the game by verifying any mine is revealed without a flag
        for (int i = 0 ; i < row ; i++)
        {
            for (int j = 0 ; j < col ; j++)
            {
                if (b [i] [j] == 10 && show [i] [j] == 1 && flags [i] [j] == 0)
                    return true;
            }
        }
        return false;
    }


    public void winScreen ()
    {
        // Initialize the win screen panel
        card5 = new Panel ();
        card5.setBackground (backgroundColour);

        // Create the winning button
        JButton winning = new JButton (createImageIcon ("win.png"));
        winning.setActionCommand ("won");
        winning.addActionListener (this);
        winning.setBackground (buttonColour);
        winning.setForeground (buttonText);
        winning.setBorderPainted (false);
        winning.setBorder (null);

        // Add the winning button to the panel
        card5.add (winning);

        // Add the win screen panel to the card layout
        p_card.add ("5", card5);
    }


    public void loseScreen ()
    {
        // Initialize the lose screen panel
        card6 = new Panel ();
        card6.setBackground (backgroundColour);

        // Create the losing button
        JButton losing = new JButton (createImageIcon ("lost.png"));
        losing.setActionCommand ("lost");
        losing.addActionListener (this);
        losing.setBackground (buttonColour);
        losing.setForeground (buttonText);
        losing.setBorderPainted (false);
        losing.setBorder (null);
        losing.setDoubleBuffered (true);

        // Add the losing button to the panel
        card6.add (losing);

        // Add the lose screen panel to the card layout
        p_card.add ("6", card6);
    }


    public void reset ()
    {
        // Reset the flag status
        flagOn = false;
        flagPic.setIcon (createImageIcon ("cover.png"));

        // Clear and reset all arrays for the game grid
        for (int i = 0 ; i < row ; i++)
        {
            for (int j = 0 ; j < col ; j++)
            {
                b [i] [j] = 0;
                show [i] [j] = 0;
                flags [i] [j] = 0;
            }
        }

        // Re-add mines and update neighbor counts
        addMines (f);
        neighbours ();
        redraw ();

        // Update the flag count display
        flag.setText ("Number of Flags: " + f);
    }


    public int countFlags ()
    {
        // Count the number of flags currently placed on the grid
        int flagCount = 0;
        for (int i = 0 ; i < row ; i++)
        {
            for (int j = 0 ; j < col ; j++)
            {
                if (flags [i] [j] == 1)
                {
                    flagCount++;
                }
            }
        }
        return flagCount;
    }


    public void save (String filename)
    {
        // Save the current game state to a file
        PrintWriter out;
        try
        {
            out = new PrintWriter (new FileWriter (filename));

            // Save the grid values
            for (int i = 0 ; i < row ; i++)
            {
                for (int j = 0 ; j < col ; j++)
                {
                    out.println (b [i] [j]);
                }
            }

            // Save the shown state of the grid
            for (int i = 0 ; i < row ; i++)
            {
                for (int j = 0 ; j < col ; j++)
                {
                    out.println (show [i] [j]);
                }
            }

            // Save the flag state of the grid
            for (int i = 0 ; i < row ; i++)
            {
                for (int j = 0 ; j < col ; j++)
                {
                    out.println (flags [i] [j]);
                }
            }

            // Close the file writer
            out.close ();
        }
        catch (IOException e)
        {
            System.out.println ("Error opening file " + e);
        }
    }


    public void open (String filename)
    {
        // Load the game state from a file
        BufferedReader in;
        try
        {
            in = new BufferedReader (new FileReader (filename));

            // Load the grid values
            for (int i = 0 ; i < row ; i++)
            {
                for (int j = 0 ; j < col ; j++)
                {
                    b [i] [j] = Integer.parseInt (in.readLine ());
                }
            }

            // Load the shown state of the grid
            for (int i = 0 ; i < row ; i++)
            {
                for (int j = 0 ; j < col ; j++)
                {
                    show [i] [j] = Integer.parseInt (in.readLine ());
                }
            }

            // Load the flag state of the grid
            for (int i = 0 ; i < row ; i++)
            {
                for (int j = 0 ; j < col ; j++)
                {
                    flags [i] [j] = Integer.parseInt (in.readLine ());
                }
            }

            // Close the file reader
            in.close ();
        }
        catch (IOException e)
        {
            System.out.println ("Error opening file " + e);
        }

        // Redraw the grid to reflect the loaded state
        redraw ();
    }


    public void actionPerformed (ActionEvent e)
    {
        // Get the command from the event
        String command = e.getActionCommand ();
        showStatus (command + " was selected");

        // Navigate to the main screen
        if (e.getActionCommand ().equals ("s1"))
            cdLayout.show (p_card, "1");
        // Navigate to the instructions screen
        else if (e.getActionCommand ().equals ("s2"))
            cdLayout.show (p_card, "2");
        // Navigate to the settings screen
        else if (e.getActionCommand ().equals ("s3"))
            cdLayout.show (p_card, "3");
        // Start the game if a difficulty level has been selected
        else if (e.getActionCommand ().equals ("s4"))
        {
            if (difficultySelected)
            {
                reset ();
                neighbours ();
                redraw ();
                cdLayout.show (p_card, "4");
            }
            else
            {
                JOptionPane.showMessageDialog (this, "Please select a difficulty level before proceeding.", "Error", JOptionPane.ERROR_MESSAGE);
            }
        }
        // Set the difficulty level to "Ultra Easy"
        else if (e.getActionCommand ().equals ("ez"))
        {
            f = 5;
            level.setText ("    Difficulty: Ultra Easy");
            difficultySelected = true;
            flag.setText ("Number of Flags: " + f);
        }
        // Set the difficulty level to "Easy"
        else if (e.getActionCommand ().equals ("easy"))
        {
            f = 20;
            level.setText ("    Difficulty: Easy");
            difficultySelected = true;
            flag.setText ("Number of Flags: " + f);
        }
        // Set the difficulty level to "Medium"
        else if (e.getActionCommand ().equals ("medium"))
        {
            f = 35;
            level.setText ("    Difficulty: Medium");
            difficultySelected = true;
            flag.setText ("Number of Flags: " + f);
        }
        // Set the difficulty level to "Hard"
        else if (e.getActionCommand ().equals ("hard"))
        {
            f = 50;
            level.setText ("    Difficulty: Hard");
            difficultySelected = true;
            flag.setText ("Number of Flags: " + f);
        }
        // Set the difficulty level to "Ultra Hard"
        else if (e.getActionCommand ().equals ("vhard"))
        {
            f = 65;
            level.setText ("    Difficulty: Ultra Hard");
            difficultySelected = true;
            flag.setText ("Number of Flags: " + f);
        }
        // Reset the game
        else if (e.getActionCommand ().equals ("reset"))
        {
            reset ();
        }
        // Save the current game state
        else if (e.getActionCommand ().equals ("save"))
        {
            save ("save.txt");
        }
        // Load a saved game state
        else if (e.getActionCommand ().equals ("savegame"))
        {
            open ("save.txt");
        }
        // Handle the win screen button action
        else if (e.getActionCommand ().equals ("won"))
        {
            cdLayout.show (p_card, "1");
            reset ();
        }
        // Handle the lose screen button action
        else if (e.getActionCommand ().equals ("lost"))
        {
            cdLayout.show (p_card, "1");
            reset ();
        }
        // Navigate to the settings screen
        else if (e.getActionCommand ().equals ("settings"))
        {
            cdLayout.show (p_card, "3");
        }
        // Navigate to the instructions screen
        else if (e.getActionCommand ().equals ("instruct"))
        {
            cdLayout.show (p_card, "2");
        }
        // Toggle the flag status
        else if (e.getActionCommand ().equals ("flag"))
        {
            if (flagOn == true)
            {
                flagOn = false;
                flagPic.setIcon (createImageIcon ("cover.png"));
            }
            else
            {
                flagOn = true;
                flagPic.setIcon (createImageIcon ("flag.png"));
            }
        }
        // Handle tile click actions
        else
        {
            int n = Integer.parseInt (e.getActionCommand ());
            int i = n / col;
            int j = n % col;
            open (i, j);
            show [i] [j] = 1;
            redraw ();

            // If flag mode is on, place or remove a flag
            if (flagOn)
            {
                if (flags [i] [j] == 1)
                {
                    flags [i] [j] = 0;
                    show [i] [j] = 0;
                }
                else
                {
                    if (countFlags () < f)
                    {
                        flags [i] [j] = 1;
                        show [i] [j] = 1;
                    }
                    else
                    {
                        JOptionPane.showMessageDialog (this, "You have reached the maximum number of flags.", "Error", JOptionPane.ERROR_MESSAGE);
                    }
                }
            }
            else
            {
                open (i, j);
                show [i] [j] = 1;
            }
            redraw ();

            // Check if the player has won or lost
            if (win ())
            {
                cdLayout.show (p_card, "5");
            }
            else if (lose ())
            {
                cdLayout.show (p_card, "6");
            }
        }
    }


    protected static ImageIcon createImageIcon (String path)
    {
        java.net.URL imgURL = GridStarter.class.getResource (path);
        if (imgURL != null)
        {
            return new ImageIcon (imgURL);
        }


        else
        {
            System.err.println ("Couldn't find file: " + path);
            return null;
        }
    }
}


