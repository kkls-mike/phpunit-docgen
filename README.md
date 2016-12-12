# Documentation generator via PHPUnit

## Usage

Require package:

	composer require pretzlaw/phpunit-docgen

Write this in your `phpunit.xml`:

	<listeners>
        <listener class="Pretzlaw\PHPUnit\DocGen\TestCaseListener">
            <arguments>
                <string>test-evidence.md</string>
            </arguments>
        </listener>
    </listeners>

It will output all your doc comments as a nice markdown.
This brings up a nice test evidence for unit tests
or some kind of documentation when running integration tests.

## Example / Best usage

Imaigne you need to describe an API or deliver the customer some test-evidence what is guaranteed by unit testing.
Just a few doc comments can generate huge documents like this one:

	# Foo Fighters
	
	This is an API for managing Foo. It allows:
	
	- Create
	- Read
	
	## How to create a new one?
	
	Creating is easy as long as you stick to the range.
	
	### Request
	
	Do `POST /foo` with some data.
		
	## Read out all foo
	
	### Restrictions
	
	You can't see more than five. This is because ...
	
	At least one will be at the drums. Because ...

Imagine you do tests over classes or actions like this:

- tests/
  - FooController/
    - CreateTest.php
    - ShowTest.php
    - ...
  - FooControllerTest.php

### Headings and Text

Within the `FooControllerTest.php` we have:

	namespace Tests;
	
	/**
	 * Foo Fighters
	 *
	 * This is an API for managing Foo. It allows:
	 *
	 * - Create
	 * - Read
	 *
	 * @since 1994
	 */
	class FooControllerTest {}
	
This is what turns into the first section (see example doc above).
It simply puts the doc comment without `@` attributes in the output.

### Depth / Structure

Subsections will be done by parsing the namespace
and (afterwards) by the methods a test contains:

	namespace Tests\FooController;
	
	/**
	 * How to create a new one?
	 * 
	 * Creating is easy as long as you stick to the range.
	 *
	 */
	class CreateTest {
	  /**
	   * Request
	   *
	   * Do `POST /foo` with some data.
	   *
	   */
	  function testBar() {
	    $this->assertTrue( (new FooController)->create() );
	  }
	}

# Append content to another heading

When you are within one test it means you are testing one context
so sometimes you like to add documentation to some heading.
Just use the heading for all those tests again:

	/**
	 * Read out all foo
	 *
	 */
	class ShowTest {
	
	  /**
	   * Restrictions
	   *
	   * You can't see more than five. This is because ...
	   */
	  function testBar() {}
	  
	  
	  /**
	   * Restrictions
	   *
	   * At least one will be at the drums. Because ...
	   */
	  function testBaz() {}
	}


# Ignore one method or class / Do not print in doc

- Mark it with an `@internal`.
- Comments without heading or content will be ignored too.

Cheers!
