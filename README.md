# module-8-data-structures
//Default Constructor
farmingdale::linkedList::linkedList()
	:
	head(0),
	tail(0)
{}
//Copy constructor
farmingdale::linkedList::linkedList(const linkedList& copyMe)
	:
	head(0),
	tail(0)
{
	llNode* otherLLNode = copyMe.head;
	//Step 1: Traverse copyMe
	while (0 != otherLLNode) {
		//Step 2: create a new node
		llNode* temp = new llNode;
		//Step 3: copy content of the node to the traversal
		temp->data = otherLLNode->data;
		//Step 4: add new node to the end of us
		temp->next = 0;
		//if we exist, add to our tail
		if (0 != tail) {
			tail->next = temp;
		}
		if (0 == head) {
			head = temp;
		}
		tail = temp;
		otherLLNode = otherLLNode->next;
	}//traversal

}
//Destructor
farmingdale::linkedList::~linkedList() {
	//Step 1: Tranverse the list
	farmingdale::llNode* current = head;
	while (current != 0) {
		llNode* next = current->next;
		//Step 2:  Delete each node in turn
		delete current;
		current = next;
	}



}

//getHead
farmingdale::statusCode farmingdale::linkedList::getHead(std::string& returnedValue) const {
	//Step 1: If is mempty, return failure
	if (isEmpty()) {
		return FAILURE;
	}
	//Step 2: Put the head's data into returnedValue
	returnedValue = head->data;
	//Step 3: return success.
	return SUCCESS;
} // M7
//getTail
farmingdale::statusCode farmingdale::linkedList::getTail(std::string& returnedValue) const {
	//Step 1: If is empty, return failure
	if (isEmpty()) {
		return FAILURE;
	}
	//Step 2: Put the tail's data into returnedValue
	returnedValue = tail->data;
	//Step 3: return success.
	return SUCCESS;
} // M7
//insertAtHead
farmingdale::statusCode farmingdale::linkedList::insertAtHead(std::string addMe) {
	//Step 1: Allocate memory for a new node. Return failure if fails.
	llNode* temp;
	try {
		temp = new llNode;
	}
	catch (std::bad_alloc) {
		return FAILURE;
	}
	//Step 2: Assign addMe to the data field of the new node.
	temp->data = addMe;
	//Step 3: Set the next field of the new node to head.
	temp->next = head;
	//Step 4: Make head point to our new node.
	head = temp;
	//Step 5: if tail is null, then set tail to point to the new node.
	if (0 == tail) {
		tail = temp;
	}
	//Step 6: return success.
	return SUCCESS;

} // M7
//insertAtTail
farmingdale::statusCode farmingdale::linkedList::insertAtTail(std::string addMe) {
	//Step 1: Allocate a new node and return failure if we cannot allocate.
	llNode* temp;
	try {
		temp = new llNode;
	}
	catch (std::bad_alloc) {
		return FAILURE;
	}
	//Step 2: Set the data field of the new node to addME.
	temp->data = addMe;
	//Step 3: Set the next field of the new node to 0.
	temp->next = NULL;
	//Step 4: If tail is not 0.Set the tail's next field to point to the new node.
	if (NULL != tail) {
		tail->next = temp;
	}
	//Step 5: Set tail to point to new node.
	tail = temp;
	//Step 6: If head is NULL, head should point to new node.
	if (NULL == head) {
		head = temp;
	}
	//Step 7: Return success
	return SUCCESS;


} // M7
//removeAtHead
farmingdale::statusCode farmingdale::linkedList::removeAtHead(std::string& removeValue) {
	//Step 1: if the list is empty, we fail.
	if (isEmpty()) {
		return FAILURE;
	}
	//Step 2: if it's not empty, put the data from head into removeValue.
	if (!(isEmpty())) {
		removeValue = head->data;
	}
	//Step 3: Make head into the current head's next.
	llNode* current = head;
	head = head->next;
        //Step 4: if head is null, tail should be null too.
	if (NULL == head) {
		tail = NULL;
	}
	//Step 5:delete the old head.
	delete current;
        //Step 6: return success
	return SUCCESS;

} // M7

//InsertAfter(good?)
farmingdale::statusCode farmingdale::linkedList::insertAfter(llNode* insertAfterMe, std::string stringToInsert) {
//Step 1: create new node.. Fail if we cannot create.
	llNode* new_node;
	try {
		new_node = new llNode;
	}
	catch (std::bad_alloc) {
		return FAILURE;
	}
	new_node->data = stringToInsert;
	//Step 2: stitch the new node in between insertAfterMe and successor
	//take the next field of new node and put successor there
	new_node->next = insertAfterMe->next;
	//make the new node the sucessor of insertAfterMe
	insertAfterMe->next = new_node;
	//step 3: if insertafterme was the tail, we are the new tail.
	if (insertAfterMe == tail) {
		new_node = tail;
	}
	//step 4: return sucess
	return SUCCESS;

}

//Search(good)
farmingdale::llNode* farmingdale::linkedList::search(std::string found) const {
	//Step 1: traverse the list
	farmingdale::llNode* current = head;
	while (current != 0) {
		if (current->data != found) {
			 current=current->next;
			}//if
		else {
			return current;
		}//else
		}//while
	//If not found, return NULL
	return NULL;
}
//removeAtTail(good)
farmingdale::statusCode farmingdale::linkedList::removeAtTail(std::string& returnedValue) {
	//Step 1: if empty, we return Failure.
	if (isEmpty()) {
		return FAILURE;
	}
	//Step 2: traverse with trailCurrent
	llNode* current = head;
	llNode* trailCurrent = 0;
	while (0 != current && current != tail) {
		trailCurrent = current;
		current = current->next;
	}
		//Step 3: if there was a single node, set head and tail to null
		if (0==trailCurrent) {
			head = 0;
		}
		//Step 3.1: otherwise, stitch out the current from the ll.(make TC into 0)
		else {
			trailCurrent->next = 0;
			}
		tail = trailCurrent;
		//Step 4: get the string from current
		returnedValue = current->data;
		//Step 5: Delete current
		delete current;
		//Step 6: return success
		return SUCCESS;
}

//remove node******(bad)
farmingdale::statusCode farmingdale::linkedList::remove(llNode* removeMe) {
	//Step 1: do a traversal with trailCurrent and stop when current==removeMe
	llNode* current = head;
	llNode* trailCurrent = 0;
	while (current->next != NULL) {
		if (current == removeMe) {
			trailCurrent = current->next;
			current->next = current->next->next;
			delete trailCurrent;
			}
		else {
			current = current->next;
		}
	}
	//Step 2: if current is null when traversal is finished, removeME is not in the linked list and we return failure
	if (current == NULL) {
		return FAILURE;
	}
	//Step 3: if removeMe is head, then remove the head.
	if (removeMe == head) {
		delete head;
	}
	//Step 4: if current is an interior node(tail), stitch out current(trailCurrent's new successor will be current's successor.)
		if (current == tail) {
			trailCurrent->next = current->next;
		}
	//Step 5: if tail was current,trail will become trailCurrent.
		if (tail == current) {
			tail = trailCurrent;
		}
        //Step 6: delete current.
		delete current;
	//Step 7: return success
		return SUCCESS;
}
//find at position(good)
farmingdale::llNode* farmingdale::linkedList::findAtPostion(int nth) const {
	//Step 1: traverse. Stop when you've iterated nth times. Stop if current is null.
	llNode* current = head;
	int count = 0;
	while (0 != current ) {
		if (count != nth) {
			count++;
		}//if
		else {
			return current;
		}//else
		current = current->next;
		}//while
	return NULL;
}
