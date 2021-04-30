---
title: 'Contact Us'
subtitle: 'Find out more information'
section_classes: 'bg-primary-darker text-primary-lighter py-8 md:py-24'
title_text: light

form:
    name: contact
    action: '#contact-us'
    inline_errors: true
    fields:
        name:
            label: Name
            display_label: false
            placeholder: 'Your full name'
            autocomplete: 'on'
            type: text
            validate:
                required: true
        email:
            label: Email
            display_label: false
            placeholder: 'Your email address'
            type: email
            validate:
                required: true
        phone:
            label: Phone
            display_label: false
            placeholder: 'Your phone number (optional)'
            type: text
        message:
            label: Message
            display_label: false
            placeholder: 'Your message'
            type: textarea
            rows: 4
            validate:
                required: true
    buttons:
        submit:
            type: submit
            classes: no-default-style text-white bg-gray-700 hover:bg-primary 
            value: 'Submit Form'
    process:
        message: '<b>Thanks!</b> We''ve received your enquiry and will get back to you very soon...'
        reset: true
---

We would **love** to help you out with any question you may have.  Feel free to ask us anything and everything!