package myproject;

import java.util.*;  
import java.security.*;
import java.io.*;


public class MyClass{
   
    public static void main (String[] args){
    // Create a new dictionary object that contains the sentences from my words text file
    Dictionary geek = new Dictionary();
    // Get the size of the dictionary which is the number of sentences in my word text file
        int size = geek.getSize();
         
        // This values keep track of the maximum value with the int max hold the maximum number of similar characters
        int max = 0;
        // The pair of strings which give the maximum number of similar characters
        String ans1 = "";
        String ans2 = "";
       
        // Searches the list to find the optimum pair of strings
       for(int i =0; i < size; i++)
       {
      for(int j = 0; j < size; j++)
      {
      // if you do not have two equal sentences
      if(i != j)
      {
      // Get the sentence at the i'th element of the dictionary
      String sentence1 = geek.getWord(i);
    // Get the sentence at the j'th element of the dictionary
      String sentence2 = geek.getWord(j);
      // Holds the count of the similar chacters between the shares of the two sentences
      int sim_count = getSimilar(sha256(sentence1), sha256(sentence2));
      // if current count of the two sentences better the current max then update it
      if(max < sim_count)
      {
      max = sim_count;
      ans1 = sentence1;
      ans2 = sentence2;
      }
      }
     
      }
       }
       // After the search print the maximum amount found
       System.out.println("The maximum amount of similar characters in the dictionary is:\n" + max);
       System.out.println("The sentences are:\n " + ans1  + ": " + ans2);
    }

   
   
     public static String sha256(String input){
       try{
           MessageDigest mDigest = MessageDigest.getInstance("SHA-256");
           byte[] salt = "CS210+".getBytes("UTF-8");
           mDigest.update(salt);
           byte[] data = mDigest.digest(input.getBytes("UTF-8"));
           StringBuffer sb = new StringBuffer();
           for (int i=0;i<data.length;i++){
               sb.append(Integer.toString((data[i]&0xff)+0x100,16).substring(1));
           }
           return sb.toString();
       }catch(Exception e){
           return(e.toString());
       }
   }

     public static int getSimilar(String a, String b) {
    int count = 0;
   
    for(int i = 0; i < a.length(); i++)
    {
    if(a.charAt(i) == b.charAt(i)) count++;
    }
    return count;
     }
   
   
   
}



class Dictionary{
   
    private String input[];

    public Dictionary(){
        input = load("C:\\Users\\20441436\\eclipse-workspace\\myproject\\src\\myproject\\clean.txt");  
    }
   
    public int getSize(){
        return input.length;
    }
   
    public String getWord(int n){
        return input[n];
    }
   
    private String[] load(String file) {
        File aFile = new File(file);    
        StringBuffer contents = new StringBuffer();
        BufferedReader input = null;
        try {
            input = new BufferedReader( new FileReader(aFile) );
            String line = null;
            int i = 0;
            while (( line = input.readLine()) != null){
                contents.append(line);
                i++;
                contents.append(System.getProperty("line.separator"));
            }
        }catch (FileNotFoundException ex){
            System.out.println("Can't find the file - are you sure the file is in this location: "+file);
            ex.printStackTrace();
        }catch (IOException ex){
            System.out.println("Input output exception while processing file");
            ex.printStackTrace();
        }finally{
            try {
                if (input!= null) {
                    input.close();
                }
            }catch (IOException ex){
                System.out.println("Input output exception while processing file");
                ex.printStackTrace();
            }
        }
        String[] array = contents.toString().split("\n");
        for(String s: array){
            s.trim();
        }
        return array;
    }
}
