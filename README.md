/**
 * A generic node class used for a doubly-linked list.
 * @param <T> The type of element held in the node.
 */
public class Node<T> {
    
    /** A reference to the next node in the list. */
    private Node<T> next;
    
    /** The element stored in this node. */
    private T element;
    
    /** A reference to the previous node in the list. */
    private Node<T> prev;

    /**
     * Constructor that creates a node. Note the parameter order matches the DList default constructor.
     * @param next The next node in the list.
     * @param element The element to store in this node.
     * @param prev The previous node in the list.
     */
    public Node(Node<T> next, T element, Node<T> prev) {
        this.next = next;
        this.element = element;
        this.prev = prev;
    }

    // Accessor Methods
    public T getElement() { return element; }
    public Node<T> getPrev() { return prev; }
    public Node<T> getNext() { return next; }

    // Mutator Methods
    public void setElement(T element) { this.element = element; }
    public void setPrev(Node<T> prev) { this.prev = prev; }
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
	 * Add an element after a given node in the list
	 */
	@Override
	public Node<T> addAfter(Node<T> elem, T item) {
		// Constructor is Node(next, element, prev)
		Node<T> newNode = new Node<>(elem.getNext(), item, elem);
		elem.getNext().setPrev(newNode);
		elem.setNext(newNode);
		size++;
		return newNode;
	}

	/**
	 * Add an element before a given node in a list
	 */
	@Override
	public Node<T> addBefore(Node<T> elem, T item) {
		// Constructor is Node(next, element, prev)
		Node<T> newNode = new Node<>(elem, item, elem.getPrev());
		elem.getPrev().setNext(newNode);
		elem.setPrev(newNode);
		size++;
		return newNode;
	}
