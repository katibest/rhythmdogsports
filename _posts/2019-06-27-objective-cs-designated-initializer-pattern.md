---
post_author: Joel Turnbull
categories:
- Development
tags: []
post_title: Objective-C's Designated Initializer Pattern
publish_date: 2013-01-28T13:38:00.000+00:00
feature_post_image: ''
post_images: []
layout: post
current_gaslighter: false
slug: objective-cs-designated-initializer-pattern
permalink: "/blog/:slug"

---
Out of the box in Objective-C you can initialize an instance of a class by
calling alloc and init on it.

```objective-c
// Creating an instance of Party

Party *party = [[Party alloc] init];
```

Alloc allocates memory for the instance, and init gives it's instance
variables it's initial values. Both return pointers to the new instance, hence
the method chain. In this case, unless init was re-implemented in the Party
class, any instance variables in the party instance were set to 0, or the zero
pointer otherwise known as nil.

That's no good. I am of the opinion that we should always strive to create
complete objects, i.e. objects who never have nil values for their instance
variables. In Objective-C there's even a better reason for doing this. in
Objective-C a message sent to nil is silently ignored. An unexpected nil value
could create a debugging nightmare for you.

So how do we do initialize the variables? Here's one way.

```objective-c
// Setting Party values using accessors after initialization

Party *party = [[Party alloc] init];
party.date = [NSDate date];
party.location = @"Unicorn Office";
party.attendees = @[@"Joel",@"Bill",@"Peter"];
```

But that's terrible. There's danger in setting properties after the fact. The
user could fail to set the state of a vital instance variable, and as the
class designer, we're not being good citizens. I can't really put it any
better than Kent Beck does in "Smalltalk Best Practice Patterns".

"what is the first thing you want to know about a class, once you’ve decided
it may do what you want it to do? The first question is “what does it take to
create an instance?” As a class provider, you’d like the answer to this
question to be as simple as possible. With the style described above, you have
to track down references to the class and read the code before you get an
inkling of how to create a useable instance. If the code is complex, it may
take a while before you figure out what is required and what is optional in
creating an instance."

As class designers, we want to make sure there is method to represent each
valid way to create an instance. Beck describes the pattern as the "Complete
Creation Method", Objective-C calls it "Designated Initializer".

Maybe in the domain of our example, a party can be created and have a default
date if none is provided, and can have no attendees initially, but it really
must have a location or it just doesn't make sense to create it.

```objective-c
// Calling a customer Party initializer

Party *party = [[Party alloc] initWithLocation: @"Unicorn Office"];
```

It could be there are times it makes sense to initialize the other values,
date and attendees. If we provided the protocol to do so the Party.h class
interface might look like this.

```objective-c
// The Party initializers interface

#import <Foundation/Foundation.h>

@interface Party : NSObject

@property (nonatomic,strong) NSDate* date;
@property (nonatomic,strong) NSString* location;
@property (nonatomic,strong) NSArray* attendees;


-(Party*)initWithLocation:(NSString*)location;
-(Party*)initWithLocation:(NSString*)location date:(NSDate*)date;
-(Party*)initWithLocation:(NSString*)location attendees: (NSArray*)attendees;
-(Party*)initWithLocation:(NSString*)location date:(NSDate*)date attendees:(NSArray*)attendees;

@end
```

There are the four valid ways you can create a party. The trick is that only
one of these methods really sets the instance variables at the end of the day.
That method is called the Designated Initializer, and all other initializers
defer to it, directly or indirectly. Take a look at the implementation.

```objective-c
// Implementation of Party initializers deferring to a Designated Initializer

#import "Party.h"

@implementation Party

-(Party*)initWithLocation:(NSString*)location {
    return [self initWithLocation: location date:[self defaultDate] attendees:[self defaultAttendees]];
};

-(Party*)initWithLocation:(NSString*)location date:(NSDate*)date{
    return [self initWithLocation: location date:date attendees:[self defaultAttendees]];
};

-(Party*)initWithLocation:(NSString*)location attendees: (NSArray*)attendees{
    return [self initWithLocation: location date:[self defaultDate] attendees: attendees];
};

-(Party*)initWithLocation:(NSString*)location date:(NSDate*)date attendees:(NSArray*)attendees{
    self = [super init];
    self.location = location;
    self.date = date;
    self.attendees = attendees;
    return self;
};

-(NSDate*)defaultDate {
    return [NSDate date];
}

-(NSArray*)defaultAttendees {
    return @[];
}

@end
```

In Party, `initWithLocation:date:attendees:` is the Designated Initializer. It
has parameters for the most important and frequently used instance variables
of Party. It is called from all of the other initializer methods. Those
methods make use of shared default settings where values are not passed in.

It is also possible that initializers could call initializers other than the
Designated Initializer to achieve the same effect. `initWithLocation:` could
defer to `initWithLocation:date:` which would defer to
`initWithLocation:date:attendees:`. Regardless of whether `initWithLocation:`
defers to `initWithLocation:date:attendees:` directly or indirectly, all rounds
lead to the Designated Initializer.

The Designated Initializer gives us a central point where modifications to
instance creation can happen, giving us the most control over how instances
are initialized. At the end of the deferment chain, there is only one method
that is responsible for setting the initial state of the object, and a
complete object comes out every time.
