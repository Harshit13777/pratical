# pratical
Q-1
1. Simulate Cyclic Redundancy Check (CRC) error detection algorithm for noisy channel.

#include <iostream>
using namespace std;
int main(){
string pg;

cin>>pg;
string data;
cin>>data;
for(int i=0;i<pg.size()-1;i++)
{
data+="0";
}
string div=data.substr(0,pg.size());
for(int i=0;i<data.size()-pg.size()+1;i++)
{
if(div[0]=='0'){
string andiv;
andiv=div.substr(1,pg.size())+data[pg.size()+i];
div.clear();div=andiv;
continue;
}
else
{ string rediv;
for(int j=1;j<pg.size();j++)
{
if(div[j]==pg[j])
{
rediv+="0";
}
else{rediv+="1";}
}
string andiv;
andiv=rediv+data[pg.size()+i];
div.clear();div=andiv;
}
}
cout<<div;
}




 























Q-2


2. Simulate and implement stop and wait protocol for noisy channel. 

#include<iostream>
#include<stdio.h>
#include<stdlib.h>
#include<conio.h>
#include<dos.h>
using namespace std;
#define time 5
#define max_seq 1
#define tot_pack 5
int randn(int n)
{
    return rand()%n + 1;
}
typedef struct
{
       int data;
}packet;
typedef struct
{
        int kind;
        int seq;
        int ack;
        packet info;
}frame;
typedef enum{ frame_arrival,error,time_out}event_type;
frame data1;
//creating prototype
void from_network_layer(packet *);
void to_physical_layer(frame *);
void to_network_layer(packet *);
void from_physical_layer(frame*);
void sender();
void receiver();
void wait_for_event_sender(event_type *);
void wait_for_event_receiver(event_type *);
//end


#define inc(k) if(k<max_seq)k++;else k=0;


int i=1;
char turn;
int disc=0;
int main()
{
   while(!disc)
   {  sender();
     // delay(400);
      receiver();
   }
    getchar();
}
void sender()
{
     static int frame_to_send=0;
     static frame s;
     packet buffer;
     event_type event;
     static int flag=0;       //first place
     if (flag==0)
     {
   from_network_layer(&buffer);
   s.info=buffer;
   s.seq=frame_to_send;
   cout<<"\nsender information \t"<<s.info.data<<"\n";
   cout<<"\nsequence no. \t"<<s.seq;


   turn='r';
   to_physical_layer(&s);
   flag=1;
     }


 wait_for_event_sender(&event);
 if(turn=='s')
 {
     if(event==frame_arrival)
      {
      from_network_layer(&buffer);
      inc(frame_to_send);
      s.info=buffer;
      s.seq=frame_to_send;
      cout<<"\nsender information \t"<<s.info.data<<"\n";
      cout<<"\nsequence no. \t"<<s.seq<<"\n";


      getch();
      turn='r';
      to_physical_layer(&s);
      }


 }


}                   //end of sender function


void from_network_layer(packet *buffer)
{
     (*buffer).data=i;
     i++;
}                            //end of from network layer function


void to_physical_layer(frame *s)
{


     data1=*s;
}             //end of to physical layer function


void wait_for_event_sender(event_type *e)
{
      static int timer=0;
      if(turn=='s')
      {   timer++;
    //timer=0;
    return ;
  }


  else                          //event is frame arrival
    {
       timer=0;
       *e=frame_arrival;
    }


}              //end of wait for event function


void receiver()
{
     static int frame_expected=0;
     frame s,r;
     event_type event;
     wait_for_event_receiver(&event);
     if(turn=='r')
     {  if(event==frame_arrival)
    {
          from_physical_layer(&r);
          if(r.seq==frame_expected)
    {
       to_network_layer(&r.info);
       inc (frame_expected);
    }
    else
    cout<<"\nReceiver :Acknowledgement resent \n";
    getch();
    turn='s';
    to_physical_layer(&s);
      }


     }
}                     //end of receiver function




void wait_for_event_receiver(event_type *e)
{
     if(turn=='r')
     {
   *e=frame_arrival;
     }
}


void from_physical_layer(frame *buffer)
{
    *buffer=data1;
}


void to_network_layer(packet *buffer)
{
     cout<<"\nReceiver : packet received \t"<< i-1;
     cout<<"\n Acknowledgement  sent \t";
     getch();
     if(i>tot_pack)
      { disc=1;
 cout<<"\ndiscontinue\n";
      }
} 



 












Q-3
3. Simulate and implement go back n sliding window protocol

#include<iostream>
#include<ctime>
#include<cstdlib>
using namespace std;
int main()
{
 int nf,N;
 int no_tr=0;
 srand(time(NULL));
 cout<<"Enter the number of frames : ";
 cin>>nf;
 cout<<"Enter the Window Size : ";
 cin>>N;
 int i=1;
 while(i<=nf)
 {
     int x=0;
     for(int j=i;j<i+N && j<=nf;j++)
     {
         cout<<"Sent Frame "<<j<<endl;
         no_tr++;
     }
     for(int j=i;j<i+N && j<=nf;j++)
     {
         int flag = rand()%2;
         if(!flag)
             {
                 cout<<"Acknowledgment for Frame "<<j<<endl;
                 x++;
             }
         else
             {   cout<<"Frame "<<j<<" Not Received"<<endl;
                 cout<<"Retransmitting Window"<<endl;
                 break;
             }
     }
     cout<<endl;
     i+=x;
 }
 cout<<"Total number of transmissions : "<<no_tr<<endl;
 return 0;
}

 
Q-4
4. Simulate and implement selective repeat sliding window protocol. 
# include <iostream>
# include <conio.h>
# include <stdlib.h>
# include <time.h>
# include <math.h>

# define TOT_FRAMES 500
# define FRAMES_SEND 10

using namespace std;

class sel_repeat
{
 private:
  int fr_send_at_instance;
  int arr[TOT_FRAMES];
  int send[FRAMES_SEND];
  int rcvd[FRAMES_SEND];
  char rcvd_ack[FRAMES_SEND];
  int sw;
  int rw; // tells expected frame
 public:
  void input();
  void sender(int);
  void reciever(int);
};

void sel_repeat :: input()
{
 int n;  // no of bits for the frame
 int m;  // no of frames from n bits

 cout << "Enter the no of bits for the sequence number ";
 cin >> n;

 m = pow (2 , n);

 int t = 0;

 fr_send_at_instance = (m / 2);

 for (int i = 0 ; i < TOT_FRAMES ; i++)
 {
  arr[i] = t;
  t = (t + 1) % m;
 }

 for (int i = 0 ; i < fr_send_at_instance ; i++)
 {
  send[i] = arr[i];
  rcvd[i] = arr[i];
  rcvd_ack[i] = 'n';
 }

 rw = sw = fr_send_at_instance;

 sender(m);
}

void sel_repeat :: sender(int m)
{
 for (int i = 0 ; i < fr_send_at_instance ; i++)
 {
  if ( rcvd_ack[i] == 'n' )
   cout << " SENDER   : Frame " << send[i] << " is sent\n";
 }
 reciever (m);
}

void sel_repeat :: reciever(int m)
{
 time_t t;
 int f;
 int f1;
 int a1;
 char ch;
 int j=0;
 int i=0;
 srand((unsigned) time(&t));

 for (i = 0 ; i < fr_send_at_instance ; i++)
 {
  if (rcvd_ack[i] == 'n')
  {
   f = rand() % 10;

   // if = 5 frame is discarded for some reason
   // else frame is correctly recieved

   if (f != 5)
   {
    for (j = 0 ; j < fr_send_at_instance ; j++)

     if (rcvd[j] == send[i])
     {
      cout << "RECIEVER : Frame " << rcvd[j] << " recieved correctly\n";
      rcvd[j] = arr[rw];
      rw = (rw + 1) % m;
      break;
     }

    if (j == fr_send_at_instance)
     cout << "RECIEVER : Duplicate frame " << send[i] << " discarded\n";

    a1 = rand() % 5;

     // if a1 == 3 then ack is lost
     //            else recieved

    if (a1 == 3)
    {
     cout << "(Acknowledgement " << send[i] << " lost)\n";
     cout << " (SENDER TIMEOUTS --> RESEND THE FRAME)\n";
     rcvd_ack[i] = 'n';
    }
    else
    {
     cout << "(Acknowledgement " << send[i] << " recieved)\n";
     rcvd_ack[i] = 'p';
    }
   }
   else
   {
    int ld = rand() % 2;

    // if = 0 then frame damaged
    // else frame lost

    if (ld == 0)
    {
     cout << "RECIEVER : Frame " << send[i] << " is damaged\n";
     cout << "RECIEVER : Negative acknowledgement " << send[i]  << " sent\n";
    }
    else
    {
     cout << "RECIEVER : Frame " << send[i] << " is lost\n";
     cout << " (SENDER TIMEOUTS --> RESEND THE FRAME)\n";
    }
    rcvd_ack[i] = 'n';
   }
  }
 }

 for ( int j = 0 ; j < fr_send_at_instance ; j++)
 {
  if (rcvd_ack[j] == 'n')
   break;
 }

 i = 0 ;

 for (int k = j ; k < fr_send_at_instance ; k++)
 {
  send[i] = send[k];

  if (rcvd_ack[k] == 'n')
   rcvd_ack[i] = 'n';
  else
   rcvd_ack[i] = 'p';

  i++;
 }

 if ( i != fr_send_at_instance )
 {
  for ( int k = i ; k < fr_send_at_instance ; k++)
  {
   send[k] = arr[sw];
   sw = (sw + 1) % m;
   rcvd_ack[k] = 'n';
  }
 }
 cout << "Want to continue...";
 cin >> ch;
 cout << "\n";

 if (ch == 'y')
  sender(m);
 else
  exit(0);
}

int main()
{
 sel_repeat sr;
 sr.input();
 getch();
 return 0;
}





 



Q-5
5. Simulate and implement distance vector routing algorithm
#include<stdio.h>
#include<iostream>
using namespace std;                                            
struct node
{
    unsigned dist[6];
    unsigned from[6];
}DVR[10];
int main()
{
    cout<<"\n\n-------------------- Distance Vector Routing Algorithm----------- ";
    int costmat[6][6];
    int nodes, i, j, k;
    cout<<"\n\n Enter the number of nodes : ";
    cin>>nodes; //Enter the nodes
    cout<<"\n Enter the cost matrix : \n" ;
    for(i = 0; i < nodes; i++)
     {
        for(j = 0; j < nodes; j++)
        {
            cin>>costmat[i][j];
            costmat[i][i] = 0;
            DVR[i].dist[j] = costmat[i][j]; //initialise the distance equal to cost matrix
            DVR[i].from[j] = j;
        }
    }
            for(i = 0; i < nodes; i++) //We choose arbitary vertex k and we calculate the
            //direct distance from the node i to k using the cost matrix and add the distance from k to node j
            for(j = i+1; j < nodes; j++)
            for(k = 0; k < nodes; k++)
                if(DVR[i].dist[j] > costmat[i][k] + DVR[k].dist[j])
                {   //We calculate the minimum distance
                    DVR[i].dist[j] = DVR[i].dist[k] + DVR[k].dist[j];
                    DVR[j].dist[i] = DVR[i].dist[j];
                    DVR[i].from[j] = k;
                    DVR[j].from[i] = k;
                }
        for(i = 0; i < nodes; i++)
        {
            cout<<"\n\n For router: "<<i+1;
            for(j = 0; j < nodes; j++)
                cout<<"\t\n node "<<j+1<<" via "<<DVR[i].from[j]+1<<" Distance "<<DVR[i].dist[j];
        }
    cout<<" \n\n ";
    return 0;
}

 

Q-6
 6. Simulate and implement Dijkstra algorithm for shortest path routing.
#include <limits.h> 
#include <stdio.h> 
  
// Number of vertices in the graph 
#define V 9 
  
// A utility function to find the vertex with minimum distance value, from 
// the set of vertices not yet included in shortest path tree 
int minDistance(int dist[], bool sptSet[]) 
{ 
    // Initialize min value 
    int min = INT_MAX, min_index; 
  
    for (int v = 0; v < V; v++) 
        if (sptSet[v] == false && dist[v] <= min) 
            min = dist[v], min_index = v; 
  
    return min_index; 
} 
  
// A utility function to print the constructed distance array 
void printSolution(int dist[]) 
{ 
    printf("Vertex \t\t Distance from Source\n"); 
    for (int i = 0; i < V; i++) 
        printf("%d \t\t %d\n", i, dist[i]); 
} 
  
// Function that implements Dijkstra's single source shortest path algorithm 
// for a graph represented using adjacency matrix representation 
void dijkstra(int graph[V][V], int src) 
{ 
    int dist[V]; // The output array.  dist[i] will hold the shortest 
    // distance from src to 
    // Initialize all distances as INFINITE and stpSet[] as false I 
  
    bool sptSet[V]; // sptSet[i] will be true if vertex i is included in shortest
    for (int i = 0; i < V; i++) 
        dist[i] = INT_MAX, sptSet[i] = false; 
  
    // Distance of source vertex from itself is always 0 
    dist[src] = 0; 
  
    // Find shortest path for all vertices 
    for (int count = 0; count < V - 1; count++) { 
        // Pick the minimum distance vertex from the set of vertices not 
        // yet processed. u is always equal to src in the first iteration. 
        int u = minDistance(dist, sptSet); 
  
        // Mark the picked vertex as processed 
        sptSet[u] = true; 
  
        // Update dist value of the adjacent vertices of the picked vertex. 
        for (int v = 0; v < V; v++) 
  
            // Update dist[v] only if is not in sptSet, there is an edge from 
            // u to v, and total weight of path from src to  v through u is 
            // smaller than current value of dist[v] 
            if (!sptSet[v] && graph[u][v] && dist[u] != INT_MAX 
                && dist[u] + graph[u][v] < dist[v]) 
                dist[v] = dist[u] + graph[u][v]; 
    } 
  
    // print the constructed distance array 
    printSolution(dist); 
} 
  
// driver program to test above function 
int main() 
{ 
    /* Let us create the example graph discussed above */
    int graph[V][V] = { { 0, 4, 0, 0, 0, 0, 0, 8, 0 }, 
                        { 4, 0, 8, 0, 0, 0, 0, 11, 0 }, 
                        { 0, 8, 0, 7, 0, 4, 0, 0, 2 }, 
                        { 0, 0, 7, 0, 9, 14, 0, 0, 0 }, 
                        { 0, 0, 0, 9, 0, 10, 0, 0, 0 }, 
                        { 0, 0, 4, 14, 10, 0, 2, 0, 0 }, 
                        { 0, 0, 0, 0, 0, 2, 0, 1, 6 }, 
                        { 8, 11, 0, 0, 0, 0, 1, 0, 7 }, 
                        { 0, 0, 2, 0, 0, 0, 6, 7, 0 } }; 
  
    dijkstra(graph, 0); 
  
    return 0; 
}
