---
title: Code-Based UI
description: EasyClick automation scripts — Android no root — code-based UI
keywords:
 - EasyClick automation scripts Android no root code-based UI
 - UI
 - androidx
 - RecyclerView
 - EasyClick
 - mobile automation
 - automation testing
 - script development
 - Android automation
 - iOS automation
 - HarmonyOS Next
 - remote screen mirroring
 - OCR
 - PPOCR
 - YOLO
 - Cursor
 - AI programming
---



- Building UI with code
- This example demonstrates using RecyclerView from androidx

```javascript showLineNumbers
function main() {

    importPackage(androidx.recyclerview.widget);
    importPackage(android.widget)

    let recyclerView = new RecyclerView(context);
    logd(recyclerView)
    // Set layout manager
    layoutManager = new LinearLayoutManager(context);
    recyclerView.setLayoutManager(layoutManager);
    var activity = ui.getActivity();
    ui.logd("activity " + activity);
    activity.setContentView(recyclerView);
    ui.logd("activity22 " + activity);

    let giftBoxList = [];
    for (var i = 0; i < 10; i++) {
        giftBoxList.push(new GiftBox(i + "个", i * 2 + "斤", i * 3 + "袋"));
    }
    ui.logd("giftBoxList " + giftBoxList);
    let recycleAdapter = createGiftBoxAdapter(giftBoxList);
    ui.logd("recycleAdapter " + recycleAdapter);
    recyclerView.setAdapter(recycleAdapter);

}

function createGiftBoxAdapter(giftBoxList) {
    return RecyclerView.Adapter({
        onCreateViewHolder: function (parent, viewType) {
            // View creation
            let view;
            let holder;
            // Create directly or parse from XML
            view = new TextView(context);
            holder = JavaAdapter(RecyclerView.ViewHolder, {},
                view);

            return holder;
        },
        onBindViewHolder: function (holder, position) {
            // Data binding
            giftBox = giftBoxList[position];
            holder.itemView.setText(giftBox.getLollipops());
            ui.logd("giftBox " + giftBox)

        },
        getItemCount: function () {
            return giftBoxList.length;
        }

    });
}

function GiftBox(lollipops, melonSeeds, peanut) {
    this.lollipops = lollipops;
    this.melonSeeds = melonSeeds;
    this.peanut = peanut;
}

GiftBox.prototype.getLollipops = function () {
    return this.lollipops;
};
GiftBox.prototype.getMelonSeeds = function () {
    return this.melonSeeds;
};
GiftBox.prototype.getPeanut = function () {
    return this.peanut;
};
GiftBox.prototype.getGiftType = function () {
    return random(0, 1);
};

main();
```



