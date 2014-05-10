/*
 * To change this template, choose Tools | Templates
 * and open the template in the editor.
 */

package oglesby;

import java.util.*;

import java.io.*;

public class DetermineMasterworkPrice
{

private String artistFirstName;
private String artistLastName;
private String titleOfWork;
private String classification;
private Date dateOfWork;
private double height;
private double width;
private String medium;
private String subject;
private double suggestedMaximumPurchasePrice;
//BoughtPainting paint = new BoughtPainting();



    //Desc: Executes the major methods that allow a user to buy a painting.
    // First the user must input values, then the user will view the
    //suggested price. If the user chooses by buy then the bought painting file
    //will be updated, otherwise the user can go back to the main
    //menu
    //Pre: The coefficient of similarity must not be zero, otherwise
    //the user is not interested in buying the painting.
    //Post: The user will have viewed the suggested maximum price for
    //the painting they want to buy. If they chose to buy it, the files are
    //now updated accordingingly.
    public  void executeDetermineMasterworkPrice()
    {
       UserInterface ui = new UserInterface();
       BoughtPainting bp = new BoughtPainting();

        getValuesFromUser();
       
        
        if ( userBuyChoice(calculateMasterworkPrice()))
                bp.add();
        else
        ui.UserInterface();
    }

    //Desc: prompt user to input the artist first name, last name, area of
    //painting, title of work, date of work, medium and subject of painting user
    //wants to buy
    //Post: The artist first name, last name, area of painting, title of
    //work, date of work, medium and subject of painting user wants to buy have
    //values input by the user
    public  void getValuesFromUser()
    {
        //UserInterface ui = new UserInterface
    	Scanner s = new Scanner (System.in);
        System.out.println("Enter Artist First name: ");
    	artistFirstName = s.nextLine();
        System.out.println("Enter Artist Last name: ");
    	artistLastName= s.nextLine();
        System.out.println("Enter title of painting: ");
    	titleOfWork = s.nextLine();
    /*    System.out.println("Enter date painting was created: ");
    	String date = s.nextLine();
        Date dateOfWork = new Date(date); */
        System.out.println("Enter painting medium: ");
    	 medium = s.nextLine();
        System.out.println("Enter painting subject: ");
    	 subject = s.nextLine();
        System.out.println("Enter painting width: ");
        String w= s.nextLine();
        width = new Double(w);
        System.out.println("Enter painting height: ");
        String h = s.nextLine();
        height = new Double(h);


       /* BoughtPainting buy = new BoughtPainting ();
        buy.setArtistFirstName(firstName);
        buy.setArtistLastName(lastName);
        buy.setTitleOfWork(title);
        buy.setDateofWork(date);
        buy.setMedium(medium);
        buy.setSubject(subject);
        buy.setWidth(width);
        buy.setHeight(height);
        return buy;*/
    }

    //Desc: constructor for DetermineOtherWorkPrice
    //Post: allows class to set the value of all Painting fields
    // in a record
    public DetermineMasterworkPrice(String fname , String lname, String work, //Date dwork,
            String clas, String med, String sub, double h,double w,double max)
    {
		artistFirstName=fname;
		artistLastName=lname;
		titleOfWork=work;
		//Date =dwork;
		classification=clas;
		height=h;
		width=w;
		medium=med;
		subject=sub;
		suggestedMaximumPurchasePrice=max;
    

    }




    //Desc: calculate the price for the Masterwork the user wants to buy
    //Pre: there must be a most similar work and that work must have a an
    //auction purchase price, the user must have entered
    // the date of the work correctly
    //Return: the price of the Masterwork
    public double calculateMasterworkPrice()
    {
        BoughtPainting bp = new BoughtPainting();
    	double auctionPurchasePrice=bp.findPrice(artistLastName, subject, medium, height*width);
    	int currentYear=2014; //getDate()
    	int auctionYear = 1541;//dateOfWork
    	//double masterworkPrice=auctionPurchasePrice*Math.pow(8.5,((1/auctionYear)-currentYear)) + 1;
    	/*int c= determineCenturyOfCreation(auctionYear);
    	if (c==20)
    		return masterworkPrice*.25;
    	else
    		return masterworkPrice*((21-c)/(22-c));*/
         return auctionPurchasePrice;
         
    }

    //Desc: determine a date’s century of creation which is a value between 12 and 21
    //Pre: the argument must be an int value that has to be in the 4th digits
    //Return: the century of creation for the date
    public  int determineCenturyOfCreation(int i)
    {
        int century;
        if (i<1100) century=0;
        else if (i<1200)century=12;
        else if (i<1300 )century =13;
        else if (i<1400) century =14;
        else if (i<1500 )century =15;
        else if (i<1600 )century =16;
        else if (i<1700) century =17;
        else if (i<1800) century =18;
        else if (i<1900) century =19;
        else if (i<2000) century =20;
        else century=21;
        return century;
    }
        //Desc: display a value to the user and the user responds which is returned
    //      as a true or false value
    //Pre: the argument must be a double value
    //Return: a boolean value based on the user’s input
    public static boolean userBuyChoice(double d) //dont keep
    {
    	System.out.println("The price is" +d +". Do you want to buy? y/n");
    	Scanner s = new Scanner(System.in);
        String response = s.nextLine();
        //must do error checking
        if (response.equalsIgnoreCase("y")) return true;
        else return false;
    }

}



