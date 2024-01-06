# Ahmed
new repository
package server1;
//import necessary java claesses
import java.net.*;
import java.io.*;
import java.sql.*;
//define a class (SampleServer)implementing the runnable interface for concurrent execution
class SampleServer implements Runnable
{
    //run method to be executed when the thread is started
public void run()
{
 try{
        // Create a server socket to listen for client connections on port 6666
      ServerSocket ss = new ServerSocket(6666);
      //Accept a client connection to get input and send output
      Socket s = ss.accept();
      DataInputStream DIS = new DataInputStream(s.getInputStream());
      DataOutputStream DOS = new DataOutputStream(s.getOutputStream());
      //Read Civil Number and Option sent by the client
      int c1 = DIS.readInt();
      int Option = DIS.readInt();
      //make a  connection to the Derby database
      String host = "jdbc:derby://localhost:1527/Mwasalat disount";
      String un = "Ahmed";
      String pwd = "1234";
      Connection con = DriverManager.getConnection(host, un, pwd);
      Statement stmt = con.createStatement();
      //write and  SQL query to retrieve discount information based on the option sent by the client
      String sq;
      sq = "SELECT * from DISCOUNT where OPYION_ID ="+Option;
      ResultSet rs = stmt.executeQuery(sq);
      //Process the results and compute prices based on the choice selected
      while(rs.next()){
       
      int Option_id = rs.getInt("OPYION_ID");
      String Duration = rs.getString("BOOKING_DURATION");
      double DISCOUNT = rs.getDouble("DISCOUNT");
     
      int ticket_price = 20;
      int Number_of_days = 30;
      //Calculate the cost based on the selected option
      double P;
      if(Option_id == 1)
          P = ticket_price * Number_of_days;
      else if (Option_id == 2)
          P = ticket_price *Number_of_days * 6;
      else
          P = ticket_price *Number_of_days * 12;
      // Determine the discounted amount
      double ds = P * DISCOUNT;
      // Determine the final price
      double FP = P - ds;
      // Send the calculated data back to the client
      DOS.writeUTF("Option: "+Option_id );
      DOS.writeUTF(" Duration : " + Duration);
      DOS.writeUTF(" Discount :"+ DISCOUNT);
      DOS.writeDouble(P);
      DOS.writeDouble(ds);
      DOS.writeDouble(FP);
      

}
     
      //// Close the socket and server socket
      s.close();
      ss.close();
      }  
      catch(Exception e)
      {     //// Handle exceptions by printing the stack trace
          e.printStackTrace();
      }   
}
}
public class Server1 {

    
    public static void main(String[] args) {
        //Create an instance of the SampleServer class
        SampleServer ST = new SampleServer();
        //create a new thread
        Thread t = new Thread(ST);
        //start the thread
        t.start();
      
    }   }
