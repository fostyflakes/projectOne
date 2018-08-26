import java.io.File; 
import java.io.PrintWriter; 
import java.io.FileNotFoundException; 
import java.util.Scanner;
import java.util.ArrayList;

public class Main   {

	public static void main(String[] args) throws FileNotFoundException {
		
	Main mainObject = new Main(); 
	mainObject.run(); 
  }
	
	private void run () throws FileNotFoundException { 
		ArrayList<Integer> list = list();  
		ArrayList<Integer> listRNUPCOUNT = listRNUPCOUNT(list);
		ArrayList<Integer> listRNDNCOUNT = listRNDNCOUNT(list);   
		ArrayList<Integer> listRunsCount = listRunsCount(listRNDNCOUNT, listRNDNCOUNT); 
		int t = total(listRunsCount); 
	    output(t, listRunsCount); 
	}

	public ArrayList<Integer> list() throws FileNotFoundException {
		try //Exception handler
		{
			Scanner scan = new Scanner(new File("p01-in.txt")); 
		}
		catch (FileNotFoundException pExcept) 
		{
			System.out.println("Stopping. FILE NOT FOUND!"); 
			System.exit(-1); //Exit 
		}

		ArrayList<Integer> list = new ArrayList<Integer>();

		Scanner scan = new Scanner(new File("p01-in.txt")); 
		for(int i = 0; scan.hasNext() == true; i++)
		{
			i = scan.nextInt();
			list.add(i);
		}
		if(scan.hasNext() == false)
		{
			scan.close();
		}
		return list;
	}	


	public ArrayList<Integer> listRunsCount(ArrayList<Integer> listRNUPCount, ArrayList<Integer> listRNDNCOUNT) {
		ArrayList<Integer> merged = new ArrayList<Integer>();
		while(listRNUPCount.size() != listRNDNCOUNT.size()) //Checks if ups and downs arrays are of equal length
		{
			if(listRNUPCount.size() > listRNDNCOUNT.size())
			{
				listRNDNCOUNT.add(0); //increases RNDN array length if shorter
			}
			if(listRNUPCount.size() < listRNDNCOUNT.size())
			{
				listRNUPCount.add(0); //increases RNUP array length if  shorter
			}
		}
		for(int i = 0; i < listRNUPCount.size(); i++)
		{
			merged.add(listRNUPCount.get(i) + listRNDNCOUNT.get(i)); 
		}
		return merged;
	}


	public ArrayList<Integer> listRNDNCOUNT(ArrayList<Integer> list) {
		ArrayList<Integer> runs = new ArrayList<Integer>();
		int l = 0; 
		int a = list.get(0); 

		for (int i = 0; i < list.size(); i++) 
		{
			if(l >= runs.size()) 
			{
				runs.add(0);
			}
			if(i <= list.size())
			{
				int b = list.get(i); 
				if(a <= b) 
				{
					int e = runs.get(l);
					e++; 
					runs.set(l, e); 
					l = 0; 
				}
				else 
				{
					l++; 
					if(l >= runs.size())
					{
						runs.add(0);
					}
					if(i == list.size()-1) 
					{
						int e = runs.get(l);
						e++;
						runs.set(l, e); 
					}
				}
				a = b; 
			}
		} //End for loop
		return runs;
	}

	public ArrayList<Integer> listRNUPCOUNT(ArrayList<Integer> list) {
		ArrayList<Integer> runs = new ArrayList<Integer>();
		
		int l = 0; // length indicator
		int a = list.get(0); //Read first int

		for (int i = 0; i < list.size(); i++) 
		{
			if(l >= runs.size()) 
			{
				runs.add(0);
			}
			if(i <= list.size())
			{
				int b = list.get(i); //Reads second int 
				if(a >= b) //Compares last 2 int's read to determine a run end or continuation
				{
					int e = runs.get(l); //Retrieves number of runs 
					e++;
					runs.set(l, e); //Updates array position 
					l = 0; //Reset counter
				}
				else //If second int is larger than the first then run continues
				{
					l++; 
					if(l >= runs.size())
					{
						runs.add(0);
					}
					if(i >= list.size()-1) 
					{
						int e = runs.get(l);
						e++;
						runs.set(l, e); //Updates run count 
					}
				}
				a = b; //Second int becomes first int for next comparison
			}
		} //End for loop 
		return runs;
	}	
	
	public int total(ArrayList<Integer> listRunsCount) {
		{
			int sum = 0; //Sum counter
			for(int i = 1; i <listRunsCount.size(); i++) 
			{
				int t = listRunsCount.get(i); 
				sum +=t; 
			}
			   return sum;
		    }
		}
	  
    public PrintWriter output(int t, ArrayList<Integer> listRunsCount) throws FileNotFoundException {
		
		PrintWriter output = new PrintWriter(new File("p01-runs.txt"));

		output.println("Run Total  " + t); 
		for(int i = 1; i < listRunsCount.size(); i++)
			{
				output.println("runs " + i + ", " + listRunsCount.get(i)); //Print Line
			}
		output.close(); 
		return output;
	}

}
