/**
 * A generic node class used for a doubly-linked list.
 * * @param <T> The type of element held in the node.
 */
public class Node<T> {
    
    /** The element stored in this node. */
    private T element;
    
    /** A reference to the previous node in the list. */
    private Node<T> prev;
    
    /** A reference to the next node in the list. */
    private Node<T> next;

    /**
     * Constructor that creates a node with the specified previous node, element, and next node.
     * * @param prev The previous node in the list.
     * @param element The element to store in this node.
     * @param next The next node in the list.
     */
    public Node(Node<T> prev, T element, Node<T> next) {
        this.prev = prev;
        this.element = element;
        this.next = next;
    }

    /**
     * Retrieves the element stored in this node.
     * @return The element of type T.
     */
    public T getElement() { return element; }
    
    /**
     * Retrieves the previous node.
     * @return The previous Node.
     */
    public Node<T> getPrev() { return prev; }
    
    /**
     * Retrieves the next node.
     * @return The next Node.
     */
    public Node<T> getNext() { return next; }

    /**
     * Sets the element for this node.
     * @param element The new element to store.
     */
    public void setElement(T element) { this.element = element; }
    
    /**
     * Sets the previous node reference.
     * @param prev The new previous Node.
     */
    public void setPrev(Node<T> prev) { this.prev = prev; }
    
    /**
     * Sets the next node reference.
     * @param next The new next Node.
     */
    public void setNext(Node<T> next) { this.next = next; }

    /**
     * Returns a string representation of the node.
     * @return A string formatted as "<element>".
     */
    @Override
    public String toString() {
        if (element == null) {
            return "<>";
        }
        return "<" + element.toString() + ">";
    }
}
/**
 * An interface representing a List ADT.
 * * @param <T> The type of comparable elements in the list.
 */
public interface IList<T extends Comparable<T>> {
	public Node<T> addAfter(Node<T> elem, T item);
	public Node<T> addBefore(Node<T> elem, T item);
	public Node<T> addFirst(T item);
	public Node<T> addLast(T item);
	public T remove(Node<T> elem);
	public Node<T> search(T elem);
	
	public T head();
	public IList<T> tail();
	
	public boolean isEmpty();
	public Integer size();
}
import java.util.Random;

/**
 * A test class to execute and verify the DList implementation.
 */
public class Test {

	public static void main(String[] args) {
		DList<Integer> list1 = new DList<Integer>();
		DList<Integer> list2 = new DList<Integer>();
		
		System.out.println("Random Numbers and Inserts");
		Random rand = new Random();
		int item3 = 0;
		for (int i = 0; i < 5; i++) {
			int j = rand.nextInt(100);
			list1.addLast(j);
			list2.addFirst(j);
			if (i == 3) item3 = j;
		}
		
		System.out.println("List1: " + list1.toString());
		System.out.println("List2: " + list2.toString());
		System.out.println("4th item in list 1: " + item3);
		
		Node<Integer> node = list1.search(item3);
		System.out.println("Node: " + node.toString());
		
		Integer item3r = list1.remove(node);
		System.out.println("List1 without item: " + list1.toString());
		System.out.println("Removed item as expected: " + (item3r == item3));
		
		System.out.println("\nTesting AddBefore and AddAfter");
		list1 = new DList<Integer>();
		Node<Integer> n1 = list1.addFirst(1);
		list1.addAfter(n1, rand.nextInt(100));
		list1.addAfter(n1, rand.nextInt(10));
		list1.addBefore(n1, rand.nextInt(100));
		System.out.println("List: " + list1.toString());
		
		list1 = new DList<Integer>();
		list2 = new DList<Integer>();
		list1.addLast(6);
		list1.addLast(9);
		list2.addLast(2);
		list2.addLast(8);
	
		DList<Integer> list3 = list1.merge(list2);
		DList<Integer> sortedList = list3.quicksort();
		
		System.out.println("\nMerge and Sorted sort test:");
		System.out.println("List1: " + list1.toString());
		System.out.println("List2: " + list2.toString());
		System.out.println("List3 (merged): " + list3.toString());
		System.out.println("SortedList: " + sortedList.toString());
		
		System.out.println("\nSorting Random List");
		list1 = new DList<Integer>();
		for (int i = 0; i < 9; i++) {
			list1.addLast(rand.nextInt(100));
		}
		System.out.println("Unsorted List: " + list1.toString());
		System.out.println("Sorted List: " + list1.quicksort());
		
	}
}
