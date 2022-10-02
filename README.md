Question - 1: Explain what the simple List component does.

Answer- Lists are used to display data in an ordered format and mainly used to display menus on websites. In React, Lists can be created in a similar way as we create lists in JavaScript. Let us see how we transform Lists in regular JavaScript.

Question -2-What problems / warnings are there with code?

1) Syntax mistake: const [setSelectedIndex, selectedIndex] = useState();
2) items was declere as null. So map Cannot read properties of null
   WrappedListComponent.defaultProps = { items: null, };
3)onClickHandler are called directly. It should call with a callback function
  onClick={onClickHandler(index)}
  
Question 3: Please fix, optimize, and/or modify the component as much as you think is necessary.

Answer- import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected === true ? 'green' : 'red' }}

      onClick={() => onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({
  items,
}) => {

  const [newItems, setNewItems,] = useState([]);

  useEffect(() => {
    setNewItems(items);
  }, [items]);

  const handleClick = (index) => {
    let newArray = [...newItems];
    newArray[index].isSelected = !newArray[index].isSelected
    setNewItems(newArray);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {newItems.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={handleClick}
          text={item.text}
          index={index}
          isSelected={item?.isSelected}
        />
      ))}
    </ul>
  )
};

WrappedListComponent.propTypes = {

  items: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
    isSelected: PropTypes.bool,
  })),
};

WrappedListComponent.defaultProps = {
    
  items: [
    {
      text: "Test-1",
      isSelected: false,
    },
    {
      text: "Test-2",
      isSelected: true,
    },
    {
      text: "Test-3",
      isSelected: false,
    },
    {
      text: "Test-4",
      isSelected: true,
    }
  ],
};

const List = memo(WrappedListComponent);

export default List;
