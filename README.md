# final
Hossen sheik awad &amp; mhmad alaswad
@@ -1 +1,394 @@
وووووووووووووووو
/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
package uc;



import java.util.PriorityQueue;
import java.util.HashSet;
import java.util.Set;
import java.util.Collections;
import java.util.List;
import java.util.ArrayList;
import java.util.Comparator;

public class Uc {
    
    
    
    

//diff between uniform cost search and dijkstra algo is that UCS has a goal

  public static void main(String[] args){

    //initialize the graph base on the Romania map
    Node n1 = new Node("S");
    Node n2 = new Node("A");
    Node n3 = new Node("B");
    Node n4 = new Node("C");
    Node n5 = new Node("G");
  
 
    //initialize the edges

    //Arad
    n1.adjacencies = new Edge[]{
      new Edge(n2,1),
      new Edge(n3,4)
    };
     
     //Zerind
    n2.adjacencies = new Edge[]{
      new Edge(n3,3),
      new Edge(n4,6),
                        new Edge(n5,7)
    };
     

     //Oradea
    n3.adjacencies = new Edge[]{
      new Edge(n4,8)
    };
     
     //Sibiu
    n4.adjacencies = new Edge[]{
      
      new Edge(n5,10)
      
    };
     

     //Fagaras
    n5.adjacencies = new Edge[]{
      
    };
     
     
     
     
   
   
     
    
     
     

    UniformCostSearch(n1,n5);

    List<Node> path = printPath(n5);

    System.out.println("Path: " + path);

  }

  public static void UniformCostSearch(Node source, Node goal){
    
    source.pathCost = 0;
    PriorityQueue<Node> queue = new PriorityQueue<Node>(20, 
      new Comparator<Node>(){

        //override compare method
        public int compare(Node i, Node j){
          if(i.pathCost > j.pathCost){
            return 1;
          }

          else if (i.pathCost < j.pathCost){
            return -1;
          }

          else{
            return 0;
          }
        }
      }

    );
    
    queue.add(source);
    Set<Node> explored = new HashSet<Node>();
    boolean found = false;
    
    //while frontier is not empty
    do{

      Node current = queue.poll();
      explored.add(current);
      

      //end if path is found
      if(current.value.equals(goal.value)){
        found = true;

        
      }

      


      for(Edge e: current.adjacencies){
        Node child = e.target;
        double cost = e.cost;
        

        
        //add node to queue if node has not been explored
        if(!explored.contains(child) && !queue.contains(child)){
          child.pathCost = current.pathCost + cost;
          child.parent = current;
          queue.add(child);
            
        }
          

        //current path is shorter than previous path found
        else if((queue.contains(child))&&(child.pathCost>(current.pathCost+cost))){
          child.parent=current;
          child.pathCost = current.pathCost + cost;
          queue.remove(child);
          queue.add(child);
      
        }


      }

      
    }while(!queue.isEmpty()&& (found==false));

  }

  public static List<Node> printPath(Node target){
    List<Node> path = new ArrayList<Node>();
    for(Node node = target; node!=null; node = node.parent){
      path.add(node);
    }

    Collections.reverse(path);

    return path;

  }

}

class Node{

  public final String value;
  public double pathCost;
  public Edge[] adjacencies;
  public Node parent;

  public Node(String val){

    value = val;
    
  }

  public String toString(){
    return value;
  }

}

class Edge{
  public final double cost;
  public final Node target;

  public Edge(Node targetNode, double costVal){
    cost = costVal;
    target = targetNode;

  }
}
........................................................................................
package astaralgorithm;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.PriorityQueue;

 class Node implements Comparable<Node> {
      // Id for readability of result purposes
      private static int idCounter = 0;
      public int id;

      // Parent in the path
      public Node parent = null;

      public List<Edge> neighbors;

      // Evaluation functions
      public double f = Double.MAX_VALUE;
      public double g = Double.MAX_VALUE;
      // Hardcoded heuristic
      public double h; 

      Node(double h){
            this.h = h;
            this.id = idCounter++;
            this.neighbors = new ArrayList<>();
      }

      @Override
      public int compareTo(Node n) {
            return Double.compare(this.f, n.f);
      }
      
      public static class Edge {
            Edge(int weight, Node node){
                  this.weight = weight;
                  this.node = node;
            }

            public int weight;
            public Node node;
      }
      // this code for adding nieghbors for each node 
      public void addBranch(int weight, Node node){
            Edge newEdge = new Edge(weight, node);
            neighbors.add(newEdge);
      }
       //calculate heuristic value
      public double calculateHeuristic(Node target){
            return this.h;
      }
}


public class AstarAlgorithm {
    
    /*We maintain two lists of nodes, an open list and a closed list.

      The open list contains nodes that we've encountered, 
 
     but haven't analyzed yet. Initially, it only contains the starting node.

    The closed list contains nodes whose all neighbors have been added to the open list.
    
    Closed nodes have their shortest path calculated and their adjacent nodes "scheduled" for
    analysis by being added to the open list.

Closed nodes can become open again if we encounter them through a different path and 
    
    that path is more optimal than the one we previously used to reach them.

We go through open nodes, open their neighbors, calculate their f and g and then close them again.*/
    
     static Node aStar(Node start, Node target){
    PriorityQueue<Node> closedList = new PriorityQueue<>();
    PriorityQueue<Node> openList = new PriorityQueue<>();

    start.f = start.g + start.calculateHeuristic(target);
    openList.add(start);

    while(!openList.isEmpty()){
        Node n = openList.peek();
        if(n == target){
            return n;
        }

        for(Node.Edge edge : n.neighbors){
            Node m = edge.node;
            double totalWeight = n.g + edge.weight;

            if(!openList.contains(m) && !closedList.contains(m)){
                m.parent = n;
                m.g = totalWeight;
                m.f = m.g + m.calculateHeuristic(target);
                openList.add(m);
            } else {
                if(totalWeight < m.g){
                    m.parent = n;
                    m.g = totalWeight;
                    m.f = m.g + m.calculateHeuristic(target);

                    if(closedList.contains(m)){
                        closedList.remove(m);
                        openList.add(m);
                    }
                }
            }
        }

        openList.remove(n);
        closedList.add(n);
    }
    return null;
}

public static void printPath(Node target){
    Node n = target;

    if(n==null)
        return;

    List<Integer> ids = new ArrayList<>();

    while(n.parent != null){
        ids.add(n.id);
        n = n.parent;
    }
    ids.add(n.id);
    Collections.reverse(ids);

    for(int id : ids){
        System.out.print(id + " ");
    }
    System.out.println("");
}


    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
       
        //we replace the name of nodes
        //s->0
        //A->1
        //B->2
        //C->3
        //G->4
        
        
        //building Graph
        Node head = new Node(7);
        head.g = 0;
Node n1 = new Node(6);
    Node n2 = new Node(4);
    Node n3 = new Node(2);
    //adding neighbors to node 0
    head.addBranch(1, n1);
    head.addBranch(4, n2);
    
    //adding neighbors to node 1
    n1.addBranch(3, n2);
    n1.addBranch(6, n3);
    

    Node target = new Node(0);

    n1.addBranch(7, target);
    
    //adding neighbors to node 2
    n2.addBranch(8, n3);
 
    //adding neighbors to node 3
    n3.addBranch(10, target);
 
 

    Node res = aStar(head, target);
    
    //print the best solution 
    //it should print 0 1 4 which represent S A G
    printPath(res);
}
    }
    .......................................................
