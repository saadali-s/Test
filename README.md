# Testpublic class DList<T extends Comparable<T>> implements IList<T>, Cloneable {

	private Node<T> header = null;
	private Node<T> trailer = null;
	private Integer size = 0;
	
	/**
	 * Default constructor
	 */
	public DList() {
		trailer = new Node<T>(null, null, null);
		header = new Node<T>(trailer, null, null);
		trailer.setPrev(header);
		size = 0;
	}
	
	/**
	 * Construct a List from an Array
	 * @param fromArray the array used to construct the list
	 */
	public DList(T[] fromArray) {
		this(); // Initialize empty list
		for (T item : fromArray) {
			this.addLast(item);
		}
	}
	
	/**
	 * Convert the list to an array.
	 */
	@SuppressWarnings("unchecked")
	public T[] toArray() {
		T[] array = (T[]) new Comparable[size];
		int index = 0;
		Node<T> currentNode = header.getNext();
		while (currentNode != trailer) {
			array[index++] = currentNode.getElement();
			currentNode = currentNode.getNext();
		}
		return array;
	}
	
	/**
	 * Provide a deep copy of the Linked List
	 */
	@Override
	public DList<T> clone() {
		DList<T> newList = new DList<T>();
		Node<T> currentNode = header.getNext();
		while (currentNode != trailer) {
			newList.addLast(currentNode.getElement());
			currentNode = currentNode.getNext();
		}
		return newList;
	}
	
	/**
	 * Add an element after a given node in the list
	 */
	@Override
	public Node<T> addAfter(Node<T> elem, T item) {
		Node<T> newNode = new Node<>(elem, item, elem.getNext());
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
		Node<T> newNode = new Node<>(elem.getPrev(), item, elem);
		elem.getPrev().setNext(newNode);
		elem.setPrev(newNode);
		size++;
		return newNode;
	}

	/**
	 * Add an element to the start of the list
	 */
	public Node<T> addFirst(T item) {
		return addAfter(header, item);
	}
	
	/**
	 * Add an element to the end of the list
	 */
	public Node<T> addLast(T item) {
		return addBefore(trailer, item);
	}
	
	/**
	 * Remove a specified node from the list. The removed element is returned
	 */
	@Override
	public T remove(Node<T> elem) {
		T item = elem.getElement();
		elem.getPrev().setNext(elem.getNext());
		elem.getNext().setPrev(elem.getPrev());
		size--;
		return item;
	}

	/**
	 * Returns the node that contains the element that is specified as a parameter
	 */
	@Override
	public Node<T> search(T elem) {
		Node<T> currentNode = header.getNext();
		while (currentNode != trailer) {
			if (currentNode.getElement().equals(elem)) {
				return currentNode;
			}
			currentNode = currentNode.getNext();
		}
		return null;
	}

	/**
	 * Returns true if the list is empty
	 */
	@Override
	public boolean isEmpty() {
		return (header.getNext() == trailer);
	}

	/**
	 * Return the size of the list
	 */
	@Override
	public Integer size() {
		return size;
	}
	
	/**
	 * Return the first element in the list
	 */
	public T head() {
		return header.getNext().getElement();
	}
	
	/**
	 * Returns a list that contains everything except the first element
	 */
	public IList<T> tail() {
		DList<T> tailList = this.clone();
		if (!tailList.isEmpty()) {
			tailList.remove(tailList.header.getNext());
		}
		return tailList;
	}
	
	@Override
	public String toString() {
		String result = header.toString() + " <-> ";
		Node<T> currentNode = header.getNext();
		
		while (currentNode != trailer) {
			result += currentNode.toString() + " <-> ";
			currentNode = currentNode.getNext();
		}
		
		result += trailer.toString();
		return result;
	}
	
	/**
	 * Return a new list that contains all the element in the current list
	 * that are less than a specified element
	 */
	public DList<T> splitLess(T element) {
		DList<T> lessList = new DList<>();
		Node<T> currentNode = header.getNext();
		while (currentNode != trailer) {
			if (currentNode.getElement().compareTo(element) < 0) {
				lessList.addLast(currentNode.getElement());
			}
			currentNode = currentNode.getNext();
		}
		return lessList;
	}
	
	/**
	 * Return a new list that contains all the element in the current list
	 * that are greater than a specified element
	 */
	public DList<T> splitGreater(T element) {
		DList<T> greaterList = new DList<>();
		Node<T> currentNode = header.getNext();
		while (currentNode != trailer) {
			if (currentNode.getElement().compareTo(element) > 0) {
				greaterList.addLast(currentNode.getElement());
			}
			currentNode = currentNode.getNext();
		}
		return greaterList;
	}
	
	/**
	 * Return a new list that contains all the element in the current list
	 * that are equal to a specified element
	 */
	public DList<T> splitEqual(T element) {
		DList<T> equalList = new DList<>();
		Node<T> currentNode = header.getNext();
		while (currentNode != trailer) {
			if (currentNode.getElement().compareTo(element) == 0) {
				equalList.addLast(currentNode.getElement());
			}
			currentNode = currentNode.getNext();
		}
		return equalList;
	}
	
	/**
	 * Return a new IList that contains the elements merged from the current list
	 * and the passed otherList
	 * @param otherList the other list to merge
	 * @return a new list of element
	 */
	public DList<T> merge(DList<T> otherList) {
		DList<T> newList = new DList<T>();
		Node<T> currentNode = header.getNext();
		Node<T> currentNode2 = otherList.header.getNext();
		
		// Merge assuming both lists are sorted
		while (currentNode != trailer && currentNode2 != otherList.trailer) {
			if (currentNode.getElement().compareTo(currentNode2.getElement()) <= 0) {
				newList.addLast(currentNode.getElement());
				currentNode = currentNode.getNext();
			} else {
				newList.addLast(currentNode2.getElement());
				currentNode2 = currentNode2.getNext();
			}
		}
		
		// Append remaining elements from the current list
		while (currentNode != trailer) {
			newList.addLast(currentNode.getElement());
			currentNode = currentNode.getNext();
		}
		
		// Append remaining elements from otherList
		while (currentNode2 != otherList.trailer) {
			newList.addLast(currentNode2.getElement());
			currentNode2 = currentNode2.getNext();
		}
		
		return newList;
	}
	
	/**
	 * Return a new list that has been sorted using a quick sort.
	 * @return a sorted list
	 */
	public DList<T> quicksort() {
		if (size() <= 1)
			return this.clone();
		
		// Choose a "pivot" element (we'll use the first element in the list)
		T pivot = header.getNext().getElement();
		
		// Call quicksort on the list of smaller elements and greater elements
		DList<T> smaller = this.splitLess(pivot).quicksort();
		DList<T> equal = this.splitEqual(pivot);
		DList<T> greater = this.splitGreater(pivot).quicksort();
	
		//merge everything together
		DList<T> sortedList = smaller.merge(equal).merge(greater);
		return sortedList;
	}
}
