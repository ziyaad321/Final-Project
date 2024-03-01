# Final-Project


import 'package:flutter/material.dart';

void main() {
  runApp(const FriendListView());
}

class FriendListView extends StatefulWidget {
  const FriendListView({super.key});

  @override
  State<FriendListView> createState() => _FriendListViewState();
}

class _FriendListViewState extends State<FriendListView> {
  final TextEditingController friendListController = TextEditingController();
  final TextEditingController updateValueController = TextEditingController();

  List<String> friendList = [];
  String newValue = "";
  addValue() {
    friendList.add(friendListController.text);
    setState(() {
      friendList;
    });
    friendListController.clear();
  }

  deleteValue({required index}) {
    setState(() {
      friendList.removeAt(index);
    });
  }

  updateValue({required index}) {
    if (updateValueController.text.isEmpty) {}
    setState(() {
      friendList[index] = updateValueController.text;
    });
    updateValueController.clear();
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        home: Scaffold(
      appBar: AppBar(
        title: TextField(
          autofocus: true,
          keyboardType: TextInputType.text,
          controller: friendListController,
        ),
        leading: IconButton(
          onPressed: () {
            Navigator.pushNamed(context, '/');
          },
          icon: const Icon(Icons.home),
        ),
        actions: [
          IconButton(
            onPressed: () {
              addValue();
              // print(friendListController);
            },
            icon: const Icon(Icons.add),
          ),
        ],
      ),
      body: SafeArea(
        child: Column(children: [
          Container(
            color: Colors.black,
            height: 200,
            child: const Center(
              child: Text(
                "abc",
                style: TextStyle(
                  color: Colors.deepOrangeAccent,
                ),
              ),
            ),
          ),
          Expanded(
            child: ListView.builder(
                itemCount: friendList.length,
                itemBuilder: (context, index) {
                  return Container(
                    margin: const EdgeInsets.only(bottom: 2),
                    child: ListTile(
                      tileColor: Colors.amber,
                      title: Text(friendList[index]),
                      trailing: Wrap(children: [
                        IconButton(
                          onPressed: () {
                            updateValueController.text = friendList[index];
                            showDialog(
                                context: context,
                                builder: (context) {
                                  return AlertDialog(
                                    title: const Text("Update Vaue"),
                                    content: TextField(
                                      controller: updateValueController,
                                    ),
                                    actions: [
                                      ElevatedButton(
                                          onPressed: () {
                                            updateValue(index: index);
                                            Navigator.pop(context);
                                          },
                                          child: const Text("Update"))
                                    ],
                                  );
                                });
                          },
                          icon: const Icon(Icons.edit),
                        ),
                        IconButton(
                          onPressed: () {
                            showDialog(
                                context: context,
                                builder: (context) {
                                  return AlertDialog(
                                    title: const Text("Are you sure?"),
                                    content: Text(friendList[index]),
                                    actions: [
                                      ElevatedButton(
                                          onPressed: () {
                                            deleteValue(index: index);
                                            Navigator.pop(context);
                                          },
                                          child: const Text("Yes")),
                                      ElevatedButton(
                                          onPressed: () {
                                            Navigator.pop(context);
                                          },
                                          child: const Text("No"))
                                    ],
                                  );
                                });
                          },
                          icon: const Icon(Icons.delete),
                        ),
                      ]),
                    ),
                  );
                }),
          ),
        ]),
      ),
    ));
  }
}
