 6. Developa programtofindthe shortest pathbetweenverticesusingthe Bellman
Ford andvectorroutine algorithm.
 andpathvectorroutingalgorithm
 import java.util.*;
 publicclass BellmanDemoFinal
 {
 Static Scannerin=newScanner(System.in);
 public static void main(String [] args)
 {
 int V,E=1,chckNegative=0;
 intw[][]=newint[20][20];
 intedge[][]=newint[50][2];
 /*Read thenoofverticesinthegraph */
 System.out.println("Enterthenoofvertices");
 V=in.nextInt();
 System.out.println("Entertheweightmatrix");
 for(inti=1;i<=V;i++)
 for(intj=1;j<=V;j++)
 {
 w[i][j]=in.nextInt();
 if(w[i][j]!=0)
 {edge[E][0]=i;
 edge[E++][1]=j;
 }
 }
 chckNegative=bellmanFord(w,V,E,edge); if(chckNegative == 1)
 System.out.println("\nNonegativeweightcycle\n"
 ); else
 System.out.println("\nNegativeweightcycleexists\n");
 }
 publicstaticintbellmanFord(intw[][],intV,intE,intedge[][])
 {
 intu,v,S,flag=1;
 intdistance[]=newint[20];
 int parent [] = new int [20];
 /*Assignthedistanceofalltheverticesto 999 */
 for(inti=1;i<=V;i++)
 {
ComputerNetworks(BCS502)
 SMVITM,Bantaka
 distance[i]=999;
 parent[i]=-1;
 }
 System.out.println("Enterthesourcevertex");
 S =in.nextInt();
 /*Assignthedistanceofsourcevertexto 999 */
 distance[S]=0;
 /* Relaxeach edge forV-1times*/
 for(inti=1;i<=V-1;i++)
 {
 for(intk=1;k<=E;k++)
 {
 u=edge[k][0];
 v=edge[k][1];
 /*Relaxingeach edge*/
 if(distance[u]+w[u][v]<distance[v])
 {
 distance[v]=distance[u]+w[u][v];
 parent[v]=u ;
 }
 }
 }
 /* Relaxalltheedgesonemore timeto checkfornegativeweightcycle*/
 for(int k=1;k<=E;k++)
 {
 u =edge[k][0];
 v = edge[k][1] ; if(distance[u]
 +w[u][v]<distance[v]) flag = 0 ;
 }
 if(flag==1)
 for(inti=1;i<=V;i++)
 System.out.println("Vertex"+i+"->cost ="+distance[i]+"parent ="+(parent[i])); return
 f
 lag;
 }
 }
SMVITM,Bantaka
 ComputerNetworks(BCS502)
 Output:
 Enterthenoofvertices 6
 Entertheweightmatrix 0
 3 25999999
 3099914999
 2999029991
 51203999
 9994999302
 99999919992999
 Enterthesourcevertex 1
 Vertex1->cost=0 parent=-1
 Vertex2->cost=3 parent=1
 Vertex3->cost=2 parent=1
 Vertex4->cost=4 parent=2
 Vertex5->cost=5 parent=6
 Vertex6->cost=3parent=3 No
 negative weight cycle
 Enterthenoofvertices 5
 Entertheweightmatrix 0-1 4 999 999
 9990322
 9999990999999
 999150999
 999999999-30
 Enterthesourcevertex 1
 Vertex1->cost=0parent=-1
 Vertex2->cost=-1parent=1
 Vertex3->cost=2 parent=2
 Vertex4->cost=-2parent=5
 Vertex5->cost=1parent=2 No
 negative weight cycle
 Enterthenoofvertices 5
 Entertheweightmatrix 0-1 4 999 999
 9990322
 9999990999999
 999150999
 999999999-50
 Enterthesourcevertex 1
 Negativeweightcycleexist
