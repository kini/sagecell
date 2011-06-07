Compute Server Javascript
=========================

.. default-domain:: js


Dependencies
^^^^^^^^^^^^
JQuery: http://www.jquery.com

JQueryUI: http://www.jqueryui.com

CodeMirror: http://codemirror.net/


Global Functions
^^^^^^^^^^^^^^^^^

.. _uuid4:
.. function:: uuid4()
    
    Creates a UUID4-compliant identifier as specified in `RFC 4122 <http://tools.ietf.org/html/rfc4122.html>`_. `CC-by-SA-licensed <http://creativecommons.org/licenses/by-sa/2.5/>`_ from `StackOverflow <http://stackoverflow.com/questions/105034/how-to-create-a-guid-uuid-in-javascript>`_ contributers.

.. _makeClass:
.. function:: makeClass()

    Generic class constructor to instantiate objects. `MIT-licensed <http://www.opensource.org/licenses/mit-license.php>`_ by `John Resig <http://ejohn.org/blog/simple-class-instantiation/>`_. 

.. _colorize:
.. function:: colorize()

    Colorizes error tracebacks formatted with `IPython <http://ipython.scipy.org>`_'s ultraTB library.


Session Class
^^^^^^^^^^^^^

.. _Session:
.. class:: Session(selector)

    :param String selector: JQuery selector for overall session output.

Session Functions
_________________

.. _Session.clearQuery:
.. function:: Session.clearQuery()

    Ends web server querying for the session.

.. _Session.get_output:
.. function:: Session.get_output()

    Polls the web server for computation results and other messages. Calls :ref:`get_output_success() <Session.get_output_success>` when messages are returned for the session.

.. _Session.get_output_success:
.. function:: Session.get_output_success(data, textStatus, jqXHR)

    Callback function that is executed if the GET request in :ref:`get_output() <Session.get_output>` succeeds. Interprets, formats, and outputs returned message contents as user-readable HTML.

.. _Session.output:
.. function:: Session.output(html)

    Outputs content to the JQUery selector defined in :ref:`session_output <Session.session_output>`.
    
    :param String html: Html markup to be inserted.
    
.. _Session.restoreOutput:
.. function:: Session.restoreOutput()

    Resets output location for computations to its default value, sets :ref:`replace_output <Session.replace_output>` to append (rather than replace) previous output, and resets :ref:`lock_output <Session.lock_output>` to guarantee that the output location can be set. This function overrides any previous uses of :ref:`setOutput() <Session.setOutput>`.

.. _Session.send_computation_success:
.. function:: Session.send_computation_success(data, textStatus, jqXHR)
    
    Callback function that is executed if the post request in :ref:`sendMsg() <Session.sendMsg>` suceeds. Checks that the returned session ID matches the sent session ID.
    
.. _Session.sendMsg:
.. function:: Session.sendMsg(code[, id])

    Posts an "execute_request" message to the web server. Supports sending messages with custom message IDs. Calls :ref:`send_computation_sucess() <Session.send_computation_success>` if post request succeeds.
    
    :param String code: Code to be executed.
    :param id: Custom message ID.

.. _Session.setQuery:
.. function:: Session.setQuery()

    Sets web server querying for new messages for the session.

.. _Session.setOutput:
.. function:: Session.setOutput(selector[, replace, lock])
    
    Sets output location for computations.
    
    :param String location: JQuery selector for computation output within the overall session output location.
    :param Bool replace: Flag designating whether computation output should replace (true) or be appended to (false) existing output.
    :param Bool lock: Flag designating whether :ref:`setOutput() <Session.setOutput>` can change the output location.

.. _Session.updateQuery:
.. function:: Session.updateQuery(interval)

    Sets web server querying for new messages for the session at a given interval.
    
    :param Int interval: New querying interval (in milliseconds).

Session Variables
_________________

.. _Session.session_id:
.. attribute:: Session.session_id

    Unique session ID generated by :ref:`uuid4() <uuid4>`.

.. _Session.sequence:
.. attribute:: Session.sequence

    Sequence number of latest message received for the session; used to track messages across sessions and check they are being received in the correct order.

.. _Session.poll_interval:
.. attribute:: Session.poll_interval

    Interval (milliseconds) used in polling the web server for additional messages.

.. _Session.session_output:
.. attribute:: Session.session_output

    JQuery selector which controls location of computation output.

.. _Session.replace_output:
.. attribute:: Session.replace_output

    Boolean flag which determines whether output (stdout, stderr, etc.) should be appended to or replace previous output.

.. _Session.lock_output:
.. attribute:: Session.lock_output

    Boolean flag which determines whether :ref:`setOutput() <Session.setOutput>` can set the output. Note that :ref:`restoreOutput() <Session.restoreOutput>` always overrides this flag.

.. _Session.eventHandlers:
.. attribute:: Session.eventHandlers

    Tracks event handlers associated with the session.

.. _Session.interacts:
.. attribute:: Session.interacts

    Tracks interacts associated with the session.


InteractCell Class
^^^^^^^^^^^^^^^^^^

.. _InteractCell:
.. class:: InteractCell(selector, data)

    Manages the configuration, display, and state of an interact control.
    
    :param String selector: JQuery selector for the location of the interact control.
    
    :param Dict data: Configuration data, including layout and controls.

InteractCell Functions
______________________

.. _InteractCell.bindChange:
.. function:: InteractCell.bindChange(interact)

    Binds Javascript change handlers for each interact control. When a change is noticed, :ref:`getChanges() <InteractCell.getChanges>` is called to determine updated function parameters and a message is sent using :ref:`Session.sendMsg() <Session.sendMsg>` with a :ref:`custom message ID <InteractCell.msg_id>` to update the interact computation result. 
    
    :param InteractCell interact: InteractCell object.

.. _InteractCell.getChanges:
.. function:: InteractCell.getChanges()

    Gets the values of an interact's controls.
    
    :returns: Dictionary of parameters and values for a given interact.

.. _InteractCell.renderCanvas:
.. function:: InteractCell.renderCanvas()

    Renders interact controls as HTML.


InteractCell Variables
______________________

.. _InteractCell.controls:
.. attribute:: InteractCell.controls
    
    Dictionary containing data on various controls (input box, slider, etc.) in the interact.

.. _InteractCell.element:
.. attribute:: InteractCell.element

    JQuery selector for the location where the interact's controls should be rendered.

.. _InteractCell.function_code:
.. attribute:: InteractCell.function_code

    Unique function code for the interact 

.. _InteractCell.interact_id:
.. attribute:: InteractCell.interact_id

    Unique ID for the interact generated by :ref:`uuid4() <uuid4>`.

.. _InteractCell.layout:
.. attribute:: InteractCell.layout

    Dictionary containing data on the layout of the controls in :ref:`controls <InteractCell.controls>`.

.. _InteractCell.session:
.. attribute:: InteractCell.session

    :ref:`Session <Session>` object which the interact is instantiated within.

.. _InteractCell.msg_id:
.. attribute: InteractCell.msg_id

    Unique ID used to differentiate and identify interact computation results. Also used as a selector for output of interact functions.