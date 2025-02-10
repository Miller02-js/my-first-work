#!/bin/bash

# Safe Mode Recovery Tool for Jailbroken iOS
# Created by: [Your GitHub Username]
# Version: 1.0

TWEAK_PATH="/Library/MobileSubstrate/DynamicLibraries"
BACKUP_PATH="/var/mobile/TweakBackup"

echo "🛠 Safe Mode Recovery Tool 🛠"
echo "Checking latest installed tweak..."

# Check if the tweak directory exists
if [ ! -d "$TWEAK_PATH" ]; then
    echo "❌ Error: Tweak directory not found! Are you jailbroken?"
    exit 1
fi

# Get the latest modified tweak file
LATEST_TWEAK=$(ls -t "$TWEAK_PATH"/*.plist | head -n 1)

if [ -z "$LATEST_TWEAK" ]; then
    echo "✅ No new tweaks found. Your issue might be elsewhere."
    exit 0
fi

TWEAK_NAME=$(basename "$LATEST_TWEAK" .plist)

echo "🔍 Found latest tweak: $TWEAK_NAME"

# Create backup folder if not exists
mkdir -p "$BACKUP_PATH"

# Move the tweak files to backup location
echo "📦 Disabling $TWEAK_NAME..."
mv "$TWEAK_PATH/$TWEAK_NAME.plist" "$BACKUP_PATH/"
mv "$TWEAK_PATH/$TWEAK_NAME.dylib" "$BACKUP_PATH/"

echo "🔄 Respringing device..."
sbreload

echo "✅ Tweak disabled! If your device is stable now, remove the tweak permanently."
exit 0
