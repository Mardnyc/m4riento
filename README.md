// AI Photo Enhancer Web App
// Name: m4riento.ai
// Username: m4riento
// Password: m4rientopots1.

import React, { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";

export default function PhotoEnhancer() {
  const [file, setFile] = useState(null);
  const [enhanced, setEnhanced] = useState(null);
  const [loading, setLoading] = useState(false);

  const handleEnhance = async () => {
    if (!file) return;
    setLoading(true);

    // Replace with your Replicate API or backend function
    const formData = new FormData();
    formData.append("image", file);

    try {
      const res = await fetch("/api/enhance", {
        method: "POST",
        body: formData,
      });
      const data = await res.json();
      setEnhanced(data.enhancedUrl);
    } catch (err) {
      alert("Error enhancing photo");
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="min-h-screen bg-black text-white flex flex-col items-center p-6">
      <h1 className="text-4xl font-bold mb-4">m4riento.ai</h1>
      <p className="mb-6 text-gray-400">Upload a photo and watch the AI magic ✨</p>

      <Card className="w-full max-w-md">
        <CardContent className="flex flex-col gap-4">
          <Input
            type="file"
            accept="image/*"
            onChange={(e) => setFile(e.target.files[0])}
          />
          <Button onClick={handleEnhance} disabled={loading}>
            {loading ? "Enhancing..." : "Enhance Photo"}
          </Button>
        </CardContent>
      </Card>

      {enhanced && (
        <div className="mt-8">
          <h2 className="text-xl mb-2">Enhanced Image</h2>
          <img src={enhanced} alt="Enhanced" className="rounded-xl border" />
        </div>
      )}
    </div>
  );
}
